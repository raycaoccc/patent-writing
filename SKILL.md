---
name: patent-writing
description: "End-to-end patent writing assistant for CNIPA, USPTO, and EPO. Use when the user asks to \"write a patent\", \"draft patent claims\", \"search prior art\", \"patent this invention\", \"写专利\", \"撰写权利要求\", \"检索现有技术\", \"写交底书\", \"技术交底书\", or needs help with any stage of patent application — from technical disclosure documents through prior art search, claims drafting, to full specification writing. Covers invention patents (发明专利), utility models (实用新型专利), technical disclosure documents (技术交底书), and AI/ML/software patent eligibility."
version: 5.0.0
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

## Expert Review Loop (审阅修改循环)

After drafting is complete (either disclosure or full patent), **ask the user ONE TIME:**

```
┌─────────────────────────────────────────────────────────┐
│  文稿已完成。是否进行专家审阅与修改？                       │
│                                                         │
│  [Y] 是 — 自动进行3轮专家审阅并修改（无需每轮确认）        │
│  [N] 否 — 直接输出最终文件                                │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

If user selects N, skip to output. If user selects Y, **自动连续执行3轮，中间不再询问。**

### Review Process (3 Rounds, Continuous)

三轮审阅意见**合并到一个Word文档**（审阅意见_汇总.docx），包含三个章节。每轮审阅后立即修改文稿，再进入下一轮。

#### Round 1: Innovation & Prior Art Review (创新性与现有技术审查)
- **Prior art perspective (web search)**: Search for conflicting patents and papers. Severity: HIGH / MEDIUM / LOW
- **Technical expert perspective**: Evaluate feasibility, parameter fragility, implementation risks
- **Document quality**: Terminology consistency, reference completeness, format compliance
- Apply all P0 and P1 fixes to the draft immediately

#### Round 2: Consistency & Completeness Review (一致性与完整性审查)
- Verify Round 1 fixes were correctly applied
- Check innovation point consistency across all sections
- Check that SNGP-type prior art components are NOT claimed as innovations
- Verify technical completeness: parameter ranges, initialization methods, convergence criteria
- Apply remaining fixes

#### Round 3: Final Quality Assurance (终审)
- Confirm all previous round fixes are in place
- Terminology uniformity, figure completeness, prior art risk residual
- Generate final quality rating
- Produce final output file

### Review Output: Single Consolidated Document

All three rounds are saved in **one Word file** (审阅意见_汇总.docx):
```
第一章：第1轮审阅意见（创新性与现有技术）
  - 现有技术冲突审查
  - 技术可行性审查
  - 修改优先级表
  - 本轮已执行的修改清单

第二章：第2轮审阅意见（一致性与完整性）
  - 第1轮修改效果确认
  - 新发现问题
  - 本轮已执行的修改清单

第三章：第3轮审阅意见（终审）
  - 第2轮修改效果确认
  - 终审检查清单
  - 综合质量评级
  - 后续操作建议
```

### Multi-Model Comprehensive Scoring (多模型综合评分)

**审阅完成后**，使用多个模型/视角对专利进行综合评分。并行启动Agent进行评估：

1. **Claude (行业专家视角)** — 技术可行性 + 实施完整性评估
2. **Web search (模拟Kimi视角)** — 现有技术风险 + 新颖性评估
3. **用户可选：调用其他模型** — 如用户配置了Kimi、GPT等MCP或API，也可调用

评分维度（每项0-10分）：

| 维度 | 说明 |
|------|------|
| 创新性 (Novelty) | 核心创新点是否区别于现有技术 |
| 技术可行性 (Feasibility) | 方案能否实际实现并达到预期效果 |
| 保护范围 (Scope) | 权利要求/保护点的覆盖范围是否合理 |
| 文档规范性 (Compliance) | 格式、术语、附图是否符合专利局要求 |
| 现有技术规避 (Prior Art Risk) | 是否成功规避了现有技术冲突 |
| 总评 (Overall) | 加权综合评分 |

最终输出综合评分表，附在审阅意见汇总文档末尾。

### De-AI Processing (去AI化处理)

**综合评分完成后、输出最终文件之前**，必须对全文执行一次去AI化处理。激活 `writing-anti-ai` skill 对文稿进行以下处理：

1. **消除AI典型表达模式**：移除"值得注意的是"、"总而言之"、"综上所述"等AI高频套话
2. **降低句式规律性**：打破AI倾向的"首先...其次...最后..."等机械并列结构
3. **增强自然表达**：将过度正式或过度工整的表述改为更自然的技术写作风格
4. **保留技术精确性**：去AI化仅调整表达方式，不改变任何技术内容、参数、公式和术语

**注意事项：**
- 专利交底书/申请文件是正式技术文档，去AI化需适度，不能过度口语化
- 技术术语、法律用语（如"其特征在于"）不做修改
- 重点处理第3部分（背景技术）和第5部分（优点推导），这两部分最容易暴露AI写作痕迹

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
