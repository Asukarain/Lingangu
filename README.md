# Lingangu — AI 图像生成知识库

> 美的中途之旅：从视觉艺术史到 AI 生图的完整知识体系

本仓库整合了视觉艺术参考数据与 AI 图像生成的全方位知识，涵盖 Midjourney V8.1、Stable Diffusion、FLUX、DALL-E 等主流工具的提示词撰写、参数控制和工作流构建。内容从基础入门到顶尖控制，致力于帮助创作者系统性地提升生图能力。

## 目录结构

```
Lingangu/
├── README.md                                    # 项目说明
├── data/                                        # 数据表格
│   └── 500_visual_breakthrough_works.csv        # 500 部视觉突破性作品数据库
├── docs/                                        # 知识文档
│   ├── midjourney.md                            # Midjourney 深度指南（含 V8/V8.1）
│   ├── stable-diffusion.md                      # Stable Diffusion 进阶控制
│   ├── flux-dalle.md                            # FLUX 与 DALL-E 提示词技巧
│   ├── tools-comparison.md                      # 2026 年主流工具对比与选择
│   └── MJ_Image_to_Video_Mastery.md             # Midjourney 图生视频完全精通指南 ★NEW
├── prompts/                                     # AI 提示词模板（JSON 格式）
│   ├── midjourney-prompts.json                  # Midjourney 模板（含 V8 专属）
│   ├── stable-diffusion-prompts.json            # Stable Diffusion 模板
│   ├── flux-dalle-prompts.json                  # FLUX & DALL-E 模板
│   └── MJ_Video_Control_Templates.json          # Midjourney 视频控制模板 ★NEW
└── prototype/                                   # 代码原型（.html, .css 等）
    └── README.md                                # 占位说明
```

## 内容概览

### `/data` — 数据表格

`500_visual_breakthrough_works.csv` 收录了从 1992 年至今的 500 部视觉突破性影像作品，包含片名、年份、类型、摄影指导、核心视觉关键词和主要技术工具等字段，是 AI 生图创作的重要视觉参考数据库。

### `/docs` — 知识文档

| 文档 | 内容 |
|------|------|
| [midjourney.md](docs/midjourney.md) | Midjourney 完整指南，含 V8/V8.1 Alpha 新特性、个性化驱动范式、参数速查表 |
| [stable-diffusion.md](docs/stable-diffusion.md) | Stable Diffusion 进阶控制，含 ControlNet、LoRA、ComfyUI 节点工作流 |
| [flux-dalle.md](docs/flux-dalle.md) | FLUX 与 DALL-E 自然语言提示词最佳实践 |
| [tools-comparison.md](docs/tools-comparison.md) | 2026 年 8 大主流工具多维度对比与选型建议 |
| [**MJ_Image_to_Video_Mastery.md**](docs/MJ_Image_to_Video_Mastery.md) | **★ 新增** Midjourney 图生视频完全精通指南，含 V1 Video Model 全维度技术栈 |

### `/prompts` — JSON 提示词模板

所有模板均为结构化 JSON 格式，可直接复制使用或程序化调用。使用方法：

1. 打开对应工具的 JSON 文件
2. 找到适合的模板类别和具体模板
3. 复制 `prompt`（或 `positive`/`negative`）字段的内容
4. 将 `{{placeholder}}` 或 `[方括号]` 中的占位符替换为实际内容
5. 参考 `parameters` 字段设置对应的生成参数

| 模板文件 | 内容 |
|---------|------|
| [midjourney-prompts.json](prompts/midjourney-prompts.json) | Midjourney 图像提示词模板，含 V8 专属模板与参数预设 |
| [stable-diffusion-prompts.json](prompts/stable-diffusion-prompts.json) | Stable Diffusion 正/负面提示词与 ControlNet 配置 |
| [flux-dalle-prompts.json](prompts/flux-dalle-prompts.json) | FLUX & DALL-E 自然语言风格模板 |
| [**MJ_Video_Control_Templates.json**](prompts/MJ_Video_Control_Templates.json) | **★ 新增** 12 个顶尖生图师专用视频控制模板 |

**视频模板特别说明**：视频模板包含 `starting_frame_prompt`（起始帧提示词）和 `video_prompt`（视频提示词）两个独立字段，需分两步使用——先生成起始帧图片，再用视频提示词生成动画。模板覆盖产品广告、奇幻电影、社交媒体循环、角色叙事、自然物理和高级技巧六大类别。

### `/prototype` — 代码原型

此目录用于存放 HTML、CSS 等代码原型文件。后续可在此添加交互式工具或演示页面。

## 更新日志

- **2026-04-23** — ★ 新增 Midjourney 图生视频完全精通指南 + 12 个视频控制 JSON 模板
  - `docs/MJ_Image_to_Video_Mastery.md`：基础原理、参数字典、物理运动诱导、镜头语言、五大流派工作流、局限与避坑
  - `prompts/MJ_Video_Control_Templates.json`：产品广告/奇幻电影/社交媒体/角色叙事/自然物理/高级技巧六大类别
- **2026-04-23** — 合并 AI-to-photos 仓库，重新组织目录结构（/data、/prompts、/prototype）
- **2026-04-22** — 新增 Midjourney V8/V8.1 知识体系，JSON 提示词模板
- **2026-04-22** — 初始化知识库，完整覆盖四大主流工具

## 贡献与更新

本知识库基于 2026 年的最新技术生态构建。AI 生图领域发展迅速，欢迎提交 Issue 或 Pull Request 来补充新的工具特性、提示词技巧或高级控制方法。

---
*由 Manus AI 整理构建 | 最后更新：2026-04-23*
