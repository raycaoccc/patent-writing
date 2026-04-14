---
name: patent-writing
description: "End-to-end patent writing assistant for CNIPA, USPTO, and EPO. Use when the user asks to \"write a patent\", \"draft patent claims\", \"search prior art\", \"patent this invention\", \"写专利\", \"撰写权利要求\", \"检索现有技术\", \"写交底书\", \"技术交底书\", or needs help with any stage of patent application — from technical disclosure documents through prior art search, claims drafting, to full specification writing. Covers invention patents (发明专利), utility models (实用新型专利), technical disclosure documents (技术交底书), and AI/ML/software patent eligibility."
version: 4.0.0
author: Claude Scholar
tags: [Patent Writing, CNIPA, USPTO, EPO, Patent Claims, Prior Art Search, AI Patent, Technical Disclosure, Intellectual Property]
---

# Patent Writing Skill

End-to-end patent writing assistant covering **technical disclosure documents**, **prior art search**, **claims drafting**, and **specification writing** for CNIPA (China), USPTO (US), and EPO (Europe), with special focus on AI/ML/software inventions.

---

## Entry Point (Start Here — MANDATORY)

**Every time this skill is activated, complete these steps in order:**

### Step 1: Read source materials

If the user provides reference files (papers, reports, existing patents, etc.), **read them FIRST** before asking questions. Follow `references/file-io-guide.md` for multi-format file reading:
- Use PyMuPDF (`fitz`) for PDF, `python-docx` for Word, `openpyxl` for Excel, `python-pptx` for PPT
- Always run dependency check and install missing packages before reading
- Always set `sys.stdout.reconfigure(encoding='utf-8')` for Chinese output on Windows
- For images, use the Read tool directly (it supports PNG, JPG, etc.)

### Step 2: Ask document type

```
┌─────────────────────────────────────────────────────────┐
│  Patent Writing Skill — 请选择撰写类型                    │
│                                                         │
│  [1] 写完整的专利申请文件                                 │
│      （权利要求书 + 说明书 + 摘要，可直接提交）             │
│                                                         │
│  [2] 写专利技术交底书                                     │
│      （提供给专利代理人的技术方案说明材料）                  │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

### Step 3: Ask output preferences

**Before starting any writing, ask the user:**

```
┌─────────────────────────────────────────────────────────┐
│  输出设置                                                │
│                                                         │
│  输出格式：[A] Markdown  [B] Word(.docx)  [C] PDF       │
│  输出目录：请指定路径（默认：桌面）                        │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

Record the user's choices and apply them at the end. If user doesn't specify, default to **Markdown on Desktop**.

---

## File I/O Capabilities

This skill includes built-in multi-format file handling. Load `references/file-io-guide.md` for full details.

### Reading (input)

| Format | Tool | Package |
|--------|------|---------|
| PDF | PyMuPDF | `pymupdf` (import fitz) |
| Word (.doc/.docx) | python-docx | `python-docx` |
| Excel (.xlsx) | openpyxl | `openpyxl` |
| PPT (.pptx) | python-pptx | `python-pptx` |
| Images (PNG/JPG) | Read tool | built-in |
| HTML | Read tool | built-in |
| Markdown | Read tool | built-in |

**Critical: Before any file read, run the dependency check script:**
```python
import subprocess, sys
for pkg in ['pymupdf', 'python-docx', 'openpyxl', 'python-pptx']:
    subprocess.check_call([sys.executable, '-m', 'pip', 'install', pkg, '-q'],
                          stdout=subprocess.DEVNULL, stderr=subprocess.DEVNULL)
```

**Critical: Always set encoding for Windows Chinese output:**
```python
import sys
sys.stdout.reconfigure(encoding='utf-8')
```

### Writing (output)

| Target Format | Method |
|---------------|--------|
| Markdown (.md) | Write tool directly |
| Word (.docx) | Python script with python-docx (see `references/file-io-guide.md`) |
| PDF | Word → PDF via docx2pdf, or direct with reportlab |

### Format Conversion Rules

When converting from Markdown to Word/PDF:
1. **Diagrams MUST be redrawn** as PNG images using matplotlib and embedded into the output file
2. ASCII art blocks in markdown must be detected and replaced with proper figures
3. Tables must be converted to native Word/PDF tables
4. If the target file is locked (PermissionError), automatically use a suffixed filename (e.g., `_v2`)
5. After generation, verify the output file contains the expected number of embedded images

---

## Diagram Generation Rules

### General Rules
- All diagram text MUST be in Chinese (matching the document language)
- All diagrams use matplotlib with `Microsoft YaHei` or `SimHei` font
- Always set `sys.stdout.reconfigure(encoding='utf-8')` before drawing
- Every flowchart decision diamond must have clear 是/否 labels with unambiguous arrow directions
- Loop-back arrows must use right-side detour paths (not overlapping the main flow)
- Save at 200 DPI as PNG

### Disclosure Document (交底书) — BLACK AND WHITE ONLY
- Background: white
- Boxes: white fill with black border (linewidth=2)
- Arrows: black
- Text: black
- Decision diamonds: white fill with black border
- Annotations: light gray fill (#F0F0F0) with dashed black border
- NO color fills, NO colored text, NO colored arrows

### Full Patent Application — Color allowed
- Use a professional color scheme (blue/green/orange/red for different modules)
- Keep colors restrained and print-friendly

---

## Disclosure Document Mode (技术交底书)

When user selects option 2, load `references/disclosure-template.md` and follow the 7-section template.

### Disclosure Document Structure (Mandatory)

**Header:** 技术方案名称 / 技术领域 / 技术联系人 + 联系方式

**Body Sections:**

| Section | Content | Key Requirements |
|---------|---------|-----------------|
| 1. 简要说明 | 目的和基本方案 | 几句话或一小段概述 |
| 2. 术语解释 | 不常见或有歧义的术语 | 百度百科能查到的不用解释 |
| 3. 技术背景和现有技术 | 技术问题 + 现有方案 + 缺点 | 缺点需有推理过程 |
| 4. 技术方案详细阐述 | 核心内容，越详细越好 | 至少3页，须配附图 |
| 5. 优点推导 | 方案优点 + 推导过程 | 需推理，不是直接列优点 |
| 6. 关键点和欲保护点 | 方案核心要点 | 简单几句话，可不写 |
| 7. 替代方案 | 是否有其他实现方式 | 有利于扩大保护范围，可不写 |

### Disclosure Document Rules

1. 所有外文必须有中文译文
2. 全文同一事物名称统一
3. 缺点需要推理得出，不能直接下结论
4. 第4部分最重要，须配附图（黑白框图）
5. 附图中部件需有唯一标号，全文统一
6. 每幅图都需要对应的文字描述

### Disclosure Workflow

1. Read user's source materials (papers, reports, etc.)
2. Search the web for related/alternative techniques to differentiate from source
3. Identify technical field and core innovation
4. Draft all 7 sections following the template
5. Generate BLACK-AND-WHITE diagrams for Section 4
6. Review for terminology consistency
7. Convert to user's requested output format (with diagrams embedded)
8. Save to user's specified directory

---

## Mode Selection (for Full Patent Application)

When user selects option 1:

```
┌─────────────────────────────────────────────────────┐
│  完整专利 — 选择模式                                  │
│                                                     │
│  [A] 全流程    检索 → 权利要求 → 说明书全文            │
│  [B] 仅检索    仅做现有技术检索                       │
│  [C] 仅权利要求 仅撰写/优化权利要求书                  │
│  [D] 仅说明书   已有权利要求，补写说明书全文            │
│  [E] 审查润色   审查已有专利文稿，查漏补缺             │
│                                                     │
│  请同时指定：管辖区域 (CNIPA/USPTO/EPO)              │
│  和专利类型 (发明专利/实用新型/外观设计)               │
│                                                     │
└─────────────────────────────────────────────────────┘
```

### Mode A-E details

- **Mode A**: Load `references/prior-art-search.md` → `references/claims-drafting.md` → `references/cnipa-format-guide.md` → quality checklist
- **Mode B**: Load `references/prior-art-search.md` + `references/cpc-codes-guide.md`, output search report
- **Mode C**: Load `references/claims-drafting.md`, for AI/ML also load `references/ai-patent-eligibility.md`
- **Mode D**: Load `references/cnipa-format-guide.md` + `references/specification-examples.md`
- **Mode E**: Run quality checklist (see below)

### Quality Checklist (Mode E)

- [ ] 每个权利要求要素在说明书中有先行基础
- [ ] 权利要求和说明书之间术语一致
- [ ] 附图标号与正文完全匹配
- [ ] 独立权利要求仅含必要区别特征
- [ ] 从属权利要求提供有意义的缩窄
- [ ] 摘要在字数/字符限制内，无广告语言
- [ ] AI发明：权利要求展示具体技术改进
- [ ] 技术问题、技术方案、有益效果清晰陈述
- [ ] 多个实施例覆盖替代实现方式
- [ ] 附图说明与实际附图匹配
- [ ] 现有技术引用包含公开日期

---

## Quick Reference: Patent Types

| 类型 | 保护范围 | 保护期限 | 审查方式 |
|------|---------|---------|---------|
| 发明专利 | 产品、方法、用途 | 20年 | 实质审查(1-3年) |
| 实用新型专利 | 仅产品形状/结构 | 10年 | 初步审查(6-10月) |
| 外观设计专利 | 外观设计 | 15年 | 初步审查 |

## Quick Reference: AI/ML Patent Eligibility

| 管辖区域 | 测试 | 核心要求 |
|---------|------|---------|
| CNIPA | 技术三要素 | 技术问题 + 技术手段 + 技术效果 |
| USPTO | Alice/Mayo | 需展示超出抽象概念的"显著更多" |
| EPO | 技术特征 | 需具有"进一步的技术效果" |

---

## References

Load selectively based on the active mode:

- `references/file-io-guide.md` — 多格式文件读写与转换指南
- `references/disclosure-template.md` — 技术交底书模板和撰写指南
- `references/prior-art-search.md` — 检索框架、数据库、策略、报告模板
- `references/claims-drafting.md` — 权利要求结构、scope策略、AI模式
- `references/cnipa-format-guide.md` — CNIPA格式规则、分节模板、费用
- `references/ai-patent-eligibility.md` — AI/软件专利适格性对比
- `references/specification-examples.md` — 中英文说明书和权利要求范例
- `references/cpc-codes-guide.md` — AI/ML/软件CPC分类号速查
- `references/claim-examples.md` — 按技术领域的权利要求范例
