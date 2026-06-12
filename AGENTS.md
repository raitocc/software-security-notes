# AGENTS.md

This repository contains **course materials** for a Software Security course (软件安全), not source code.

## Repository Structure

```
├── 课后习题/          # After-class exercises (4 lectures)
├── 课件处理中间文件/   # Intermediate processing files
└── 课件合集/          # Courseware collection
    ├── PDF原件/       # Original PDF slides (13 files)
    ├── 复习手册/       # Review manuals with exercises
    └── 参考教材/       # Reference textbook
```

## Key Facts

- **No code to build or test** — this is educational content, not a software project
- **9 lectures** covering: basic concepts, stack/assembly, debugging, buffer overflow, other vulnerabilities, exploitation, protection/mining, web security, penetration testing
- **File types**: PDF (slides), Markdown (manuals, exercises, notes), DOCX (exercises), images

## Reading Order

Follow `课件合集/复习手册/README-使用说明.md` for the recommended study path:
1. Read Lecture01-09 review manuals in order
2. Do exercises (`LectureXX-...-习题.md`) before checking answers (`LectureXX-...-习题答案.md`)
3. Use `课件处理中间文件/` only when verifying against original PDFs

## Lecture Merging

- Lecture01-06: single PDF each
- Lecture07: merged from `lecture07part1-漏洞利用防护.pdf` + `lecture07part2-漏洞挖掘.pdf`
- Lecture08: merged from `lecture08part1` through `lecture08part4` (all Web安全)
- Lecture09: `reading-material-渗透测试.pdf` (penetration testing)

## Content Principles

- Attack demonstrations only cover principles and authorized contexts — not operation manuals
- Image-only slides are summarized as concept explanations in review manuals
- Each review manual follows: main thread → sections → self-check questions
