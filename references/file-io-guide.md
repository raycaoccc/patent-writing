# 多格式文件读写与转换指南

## 环境准备（每次执行前必须运行）

### 依赖检查与安装

```python
import subprocess, sys

PACKAGES = ['pymupdf', 'python-docx', 'openpyxl', 'python-pptx']

def ensure_deps():
    """Check and install required packages. Run ONCE at skill activation."""
    for pkg in PACKAGES:
        try:
            subprocess.check_call(
                [sys.executable, '-m', 'pip', 'install', pkg, '-q'],
                stdout=subprocess.DEVNULL, stderr=subprocess.DEVNULL
            )
        except Exception:
            pass

ensure_deps()
```

### Windows中文编码修复（必须在每个Python脚本开头执行）

```python
import sys
sys.stdout.reconfigure(encoding='utf-8')
```

不加这行，Windows终端输出中文会乱码。**每个Python脚本的第一行有效代码都必须是这个。**

---

## 文件读取

### PDF读取

```python
import fitz  # pymupdf
doc = fitz.open(r'path/to/file.pdf')
print(f'Total pages: {len(doc)}')
for i in range(len(doc)):
    page = doc[i]
    text = page.get_text()
    print(f'=== PAGE {i+1} ===')
    print(text)
```

注意：如果路径中含有中文，使用原始字符串 `r'...'`。

### Word读取

```python
from docx import Document
doc = Document(r'path/to/file.docx')

# 读取段落
for p in doc.paragraphs:
    print(p.text)

# 读取表格
for table in doc.tables:
    for row in table.rows:
        for cell in row.cells:
            print(cell.text, end=' | ')
        print()
```

### Excel读取

```python
from openpyxl import load_workbook
wb = load_workbook(r'path/to/file.xlsx')
ws = wb.active
for row in ws.iter_rows(values_only=True):
    print(row)
```

### PPT读取

```python
from pptx import Presentation
prs = Presentation(r'path/to/file.pptx')
for slide in prs.slides:
    for shape in slide.shapes:
        if shape.has_text_frame:
            for para in shape.text_frame.paragraphs:
                print(para.text)
```

### 图片读取

直接使用Claude Code的Read工具读取PNG/JPG，它原生支持图片显示。

---

## Markdown → Word转换

### 核心逻辑

1. 逐行解析Markdown
2. 检测标题（#/##/###/####）→ Word标题样式
3. 检测表格（|开头）→ Word原生表格
4. 检测代码块（```）→ 判断是否为ASCII图
   - 如果包含框线字符（┌└│├─▼▶→），视为**ASCII图**，替换为对应的PNG图片
   - 否则视为普通代码块，用等宽字体渲染
5. 检测加粗（**text**）→ Word加粗
6. 其他 → 普通段落

### ASCII图检测标准

以下任一字符出现在代码块中，即判定为ASCII图：
```
┌ └ ┘ ┐ │ ├ ┤ ─ ┬ ┴ ▼ ▶ ◆ → ← ↑ ↓
```

### 图片匹配策略

在代码块之前的文本中搜索"图N："或"图N："（全角/半角冒号），N为阿拉伯数字。找到后与预生成的PNG文件匹配：`fig{N}_*.png`。

### Word格式设置

```python
from docx import Document
from docx.shared import Pt, Cm, Inches
from docx.oxml.ns import qn

doc = Document()
style = doc.styles['Normal']
style.font.name = '宋体'
style.font.size = Pt(12)
style.element.rPr.rFonts.set(qn('w:eastAsia'), '宋体')

for sec in doc.sections:
    sec.top_margin = Cm(2.54)
    sec.bottom_margin = Cm(2.54)
    sec.left_margin = Cm(3.17)
    sec.right_margin = Cm(3.17)
```

### 图片嵌入

```python
p = doc.add_paragraph()
p.alignment = WD_ALIGN_PARAGRAPH.CENTER
run = p.add_run()
run.add_picture(fig_path, width=Inches(5.5))
```

### PermissionError处理

```python
import os

def safe_save(doc, path):
    """Save with automatic fallback if file is locked."""
    try:
        doc.save(path)
        return path
    except PermissionError:
        base, ext = os.path.splitext(path)
        for i in range(2, 10):
            alt = f"{base}_v{i}{ext}"
            try:
                doc.save(alt)
                print(f"原路径被占用，已保存到: {alt}")
                return alt
            except PermissionError:
                continue
        raise RuntimeError(f"无法保存文件，请关闭占用的程序: {path}")
```

### 生成后验证

```python
def verify_docx(path, expected_images):
    """Verify the generated docx contains expected number of images."""
    doc = Document(path)
    img_count = sum(1 for rel in doc.part.rels.values() if 'image' in rel.reltype)
    print(f'验证：文档包含 {img_count} 张图片（预期 {expected_images} 张）')
    if img_count != expected_images:
        print('⚠ 警告：图片数量不匹配！')
    return img_count == expected_images
```

---

## 图表生成

### matplotlib通用设置

```python
import matplotlib
matplotlib.use('Agg')
import matplotlib.pyplot as plt

plt.rcParams['font.sans-serif'] = ['Microsoft YaHei', 'SimHei']
plt.rcParams['axes.unicode_minus'] = False
```

### 交底书模式：黑白配色

```python
# 黑白配色常量
BW_BLACK = '#000000'
BW_WHITE = '#FFFFFF'
BW_GRAY = '#F0F0F0'      # 注释框浅灰填充
BW_BORDER = '#333333'     # 边框色

def bw_box(ax, x, y, w, h, text, fs=11):
    """黑白模式的方框"""
    from matplotlib.patches import FancyBboxPatch
    b = FancyBboxPatch((x-w/2, y-h/2), w, h, boxstyle="round,pad=0.12",
                       facecolor=BW_WHITE, edgecolor=BW_BLACK, lw=2, zorder=2)
    ax.add_patch(b)
    ax.text(x, y, text, ha='center', va='center', fontsize=fs,
            color=BW_BLACK, fontweight='bold', zorder=3)

def bw_diamond(ax, x, y, w, h, text, fs=11):
    """黑白模式的判断菱形"""
    diamond = plt.Polygon([[x, y+h/2], [x+w/2, y], [x, y-h/2], [x-w/2, y]],
                          facecolor=BW_WHITE, edgecolor=BW_BLACK, lw=2, zorder=2)
    ax.add_patch(diamond)
    ax.text(x, y, text, ha='center', va='center', fontsize=fs,
            color=BW_BLACK, fontweight='bold', zorder=3)

def bw_arrow(ax, x1, y1, x2, y2, lw=2):
    """黑白模式的箭头"""
    ax.annotate('', xy=(x2, y2), xytext=(x1, y1),
                arrowprops=dict(arrowstyle='->', color=BW_BLACK, lw=lw), zorder=1)

def bw_note(ax, x, y, w, h, text, fs=9):
    """黑白模式的虚线注释框"""
    from matplotlib.patches import FancyBboxPatch
    b = FancyBboxPatch((x-w/2, y-h/2), w, h, boxstyle="round,pad=0.08",
                       facecolor=BW_GRAY, edgecolor=BW_BLACK, lw=1.5,
                       linestyle='--', zorder=2)
    ax.add_patch(b)
    ax.text(x, y, text, ha='center', va='center', fontsize=fs,
            color=BW_BLACK, zorder=3)
```

### 全彩模式（完整专利用）

```python
C_PRIMARY = '#2C3E50'
C_BLUE = '#3498DB'
C_RED = '#E74C3C'
C_GREEN = '#2ECC71'
C_ORANGE = '#F39C12'
C_TEAL = '#1ABC9C'
C_BG = '#ECF0F1'
C_WHITE = '#FFFFFF'
```

### 流程图箭头逻辑规范

判断菱形的是/否箭头必须遵循：
- **"是"**：沿主流程方向继续（通常向下），使用绿色/正常色
- **"否"**：走分支路径（通常向右侧绕回），使用红色/异常色
- Loop-back（循环）箭头：从菱形右侧水平引出 → 垂直上行 → 水平回到目标步骤右侧入口
- 每个箭头的是/否标签放在箭头起点附近，不能离太远

### 保存规范

```python
fig.savefig(output_path, dpi=200, bbox_inches='tight', facecolor='white')
plt.close()
```

---

## 完整转换流程（Markdown → Word）

```
1. 读取Markdown源文件
2. 解析所有代码块，识别ASCII图
3. 为每个ASCII图生成对应PNG（黑白或彩色）
4. 保存PNG到输出目录/figures/子目录
5. 逐行解析Markdown，构建Word文档
6. 遇到ASCII图代码块时，插入对应PNG
7. safe_save保存Word文件（处理PermissionError）
8. verify_docx验证图片嵌入数量
```
