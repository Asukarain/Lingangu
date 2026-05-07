# Lingangu — AI 图像生成知识库

> 美的中途之旅：从视觉艺术史到 AI 生图的完整知识体系

本仓库整合了视觉艺术参考数据与 AI 图像生成的全方位知识，涵盖 Midjourney V8.1、Stable Diffusion、FLUX、DALL-E 以及最新的 **Nano Banana 2 (Gemini 3.1 Flash Image)** 等主流工具的提示词撰写、参数控制和工作流构建。内容从基础入门到顶尖控制，致力于帮助创作者系统性地提升生图能力。

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
│   ├── MJ_Image_to_Video_Mastery.md             # Midjourney 图生视频完全精通指南
│   └── nano_banana/
│       └── Mastery_Guide.md                     # ★ Nano Banana 2 完全精通指南
├── prompts/                                     # AI 提示词模板（JSON 格式）
│   ├── midjourney-prompts.json                  # Midjourney 模板（含 V8 专属）
│   ├── stable-diffusion-prompts.json            # Stable Diffusion 模板
│   ├── flux-dalle-prompts.json                  # FLUX & DALL-E 模板
│   ├── MJ_Video_Control_Templates.json          # Midjourney 视频控制模板
│   └── nano_banana/
│       └── templates.json                       # ★ Nano Banana 2 专属 JSON 模板
├── prototype/                                   # 代码原型（.html, .css 等）
│   └── README.md                                # 占位说明
└── web_app/                                     # ★ 交互式知识库网页应用
    ├── index.html                               # 主页面（含完整 React SPA）
    ├── nano_banana_lab.html                     # ★ Nano Banana Lab 专项板块
    ├── style.css                                # 样式表（Tailwind CSS 编译产物）
    └── script.js                                # 应用逻辑（React + Recharts 图表）
```

## 项目架构图

```
┌─────────────────────────────────────────────────────────────────┐
│                    Lingangu 知识库架构                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────────┐   │
│  │  /data    │  │  /docs   │  │ /prompts │  │  /prototype  │   │
│  │          │  │          │  │          │  │              │   │
│  │ CSV 数据  │  │ MD 文档  │  │ JSON 模板│  │  代码原型    │   │
│  │ 表格     │  │ 知识库   │  │ 提示词   │  │  HTML/CSS    │   │
│  └────┬─────┘  └────┬─────┘  └────┬─────┘  └──────────────┘   │
│       │             │             │                             │
│       └─────────────┼─────────────┘                             │
│                     ▼                                           │
│            ┌────────────────┐                                   │
│            │   /web_app     │                                   │
│            │                │                                   │
│            │  交互式网页应用 │                                   │
│            │  ┌────────────┐│                                   │
│            │  │ index.html ││  ← React SPA 单页应用             │
│            │  │ style.css  ││  ← Tailwind CSS 样式              │
│            │  │ script.js  ││  ← 交互图表 + 动画                │
│            │  └────────────┘│                                   │
│            └────────────────┘                                   │
│                                                                 │
│  功能板块：                                                      │
│  ┌─────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐           │
│  │Midjourney│ │V8 Future │ │Video Lab │ │Stable    │           │
│  │深度指南  │ │前瞻分析  │ │图生视频  │ │Diffusion │           │
│  └─────────┘ └──────────┘ └──────────┘ └──────────┘           │
│  ┌─────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐           │
│  │FLUX &   │ │JSON 模板 │ │工具对比  │ │成长路线  │           │
│  │DALL-E   │ │一键复制  │ │雷达/柱状 │ │五级体系  │           │
│  └─────────┘ └──────────┘ └──────────┘ └──────────┘           │
│  ┌─────────────────────────────────────────────────┐           │
│  │             ★ Nano Banana Lab ★                │           │
│  │   (Gemini 3.1 Flash Image 深度调研与实战集成)     │           │
│  └─────────────────────────────────────────────────┘           │
└─────────────────────────────────────────────────────────────────┘
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
| [MJ_Image_to_Video_Mastery.md](docs/MJ_Image_to_Video_Mastery.md) | Midjourney 图生视频完全精通指南，含 V1 Video Model 全维度技术栈 |
| [**Mastery_Guide.md**](docs/nano_banana/Mastery_Guide.md) | ★ **Nano Banana 2 完全精通指南**，含官方技术解析与社交媒体实战技巧 |

### `/prompts` — JSON 提示词模板

所有模板均为结构化 JSON 格式，可直接复制使用或程序化调用。

| 模板文件 | 内容 |
|---------|------|
| [midjourney-prompts.json](prompts/midjourney-prompts.json) | Midjourney 图像提示词模板，含 V8 专属模板与参数预设 |
| [stable-diffusion-prompts.json](prompts/stable-diffusion-prompts.json) | Stable Diffusion 正/负面提示词与 ControlNet 配置 |
| [flux-dalle-prompts.json](prompts/flux-dalle-prompts.json) | FLUX & DALL-E 自然语言风格模板 |
| [MJ_Video_Control_Templates.json](prompts/MJ_Video_Control_Templates.json) | 12 个顶尖生图师专用视频控制模板 |
| [**templates.json**](prompts/nano_banana/templates.json) | ★ **Nano Banana 2 专属模板**，含多图融合、局部编辑与复杂语义解析 |

### `/web_app` — 交互式知识库网页应用

基于 React + Tailwind CSS + Recharts 构建的单页应用，采用瑞士国际主义设计风格（Swiss International Style）。

| 板块 | 内容 |
|------|------|
| Midjourney 深度指南 | 提示词结构公式 + 18 项参数速查表 |
| V8/V8.1 新特性 | 范式转变对比、三步推荐工作流、6 组参数预设卡片 |
| **V8 Future** | 技术趋势前瞻、V9 架构预测、生态系统扩展 |
| **Video Lab** | 6 种运动类型的 Prompt→运动逻辑 卡片、视频参数速查 |
| **Nano Banana Lab** | ★ **专项板块**：极速生成、主体一致性、精准文本渲染、JSON 模板库 |
| 工具对比 | 交互式雷达图 + 柱状图 + 综合对比表 |

**本地预览方式**：直接在浏览器中打开 `web_app/index.html` 或 `web_app/nano_banana_lab.html` 即可查看。

## 更新日志

- **2026-05-07** — ★ **Nano Banana 2 专项集成**：新增 Mastery_Guide.md、JSON 模板库及 Nano Banana Lab 网页板块
- **2026-04-23** — ★ 新增交互式知识库网页应用（/web_app），含 V8 Future 前瞻板块和 Video Lab 图生视频板块
- **2026-04-23** — 新增 Midjourney 图生视频完全精通指南 + 12 个视频控制 JSON 模板
- **2026-04-23** — 合并 AI-to-photos 仓库，重新组织目录结构
- **2026-04-22** — 新增 Midjourney V8/V8.1 知识体系，JSON 提示词模板
- **2026-04-22** — 初始化知识库，完整覆盖四大主流工具

## 贡献与更新

本知识库基于 2026 年的最新技术生态构建。AI 生图领域发展迅速，欢迎提交 Issue 或 Pull Request 来补充新的工具特性、提示词技巧或高级控制方法。

---
*由 Manus AI 整理构建 | 最后更新：2026-05-07*
