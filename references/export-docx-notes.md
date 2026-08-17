# 导出 Word 操作手册（export_docx.py）

执行 SKILL.md 第 6 步「导出 Word」时读本文件。JSON 字段契约以 `scripts/export_docx.py` 顶部文档字符串为准——**编 JSON 前先读它**。

## 命令用法

```bash
python scripts/export_docx.py spec.json          # 按 JSON 里的 output 落盘
python scripts/export_docx.py spec.json -o x.docx # 覆盖输出路径
python scripts/export_docx.py --demo demo.json    # 生成示例 JSON 参考
```

- 正文块类型：`text`/`h1`/`h2`/`h3`/`plain`、`red_header`/`brief_header`/`signer`/`wenhao`/`title`/`zhusong`/`signoff_*`/`fuzhu` 等，完整清单见脚本顶部文档字符串。
- 脚本路径以本 skill 根目录为基准，从其他目录调用时写全路径。

## 字体层级缺省（经实际发文核校，GB/T 9704 标准层级）

| 要素 | 字体 | 字号 |
|---|---|---|
| 标题 | 方正小标宋简体（加粗） | 二号 |
| 一级标题 (h1) | 黑体 | 三号 |
| 二级标题 (h2) | 楷体_GB2312 | 三号 |
| 三级标题 (h3) | 仿宋_GB2312（加粗） | 三号 |
| 正文 | 仿宋_GB2312 | 三号 |

单位另有要求时在 JSON 的 `fonts` 里覆盖；本机未装的字体 Word 会自动替换，不影响生成。

## 依赖与 python 不可用时的替代调用

- 依赖 `python-docx`（`pip install -r scripts/requirements.txt`）。
- 命令中的 `python` 若不可用或**静默失败**（退出码非 0 且无输出，典型如 Windows 商店占位 stub），改用 `uv run --with python-docx python`（推荐，无需预装依赖）或 `py -3`。
- 生成后 `【待补】` 占位仍保留在 docx 中，提示用户填定后方可发文。

## 可移植性与降级（跨 agent 平台运行时）

本 skill 的**核心写作能力零依赖**——SKILL.md 第 1–5 步只产出 markdown/纯文本，任何 Agent Skills 兼容的 harness 都能跑。第 6 步导出 Word 是**可选能力**：

- 若运行环境**有** `python-docx` → 调 `export_docx.py` 出 .docx。
- 若**无法运行**（python-docx 未装、python 不可用或调用静默失败、无子进程权限）→ **降级**：照常交付 markdown/纯文本成稿 + 版式说明，并提示用户"本环境未装 python-docx，如需 Word 请 pip/uv 安装 python-docx 后重跑第 6 步，或把成稿贴入公文模板"。不因导出不可用而阻断主体交付。

## 导出后随稿提示（必做）

列出 docx **未包含**的要素：抄送、版记、页码、份号/密级、印章，注明由办公室套模板补全，防止用户误以为 Word 版要素齐全。
