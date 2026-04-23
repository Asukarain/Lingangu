# Midjourney 图生视频（Image-to-Video）完全精通指南

> **版本**：2026 年 4 月 | **作者**：Manus AI  
> **覆盖范围**：V1 Video Model + V8/V8.1 图像模型协同 + 关键帧功能

---

## 一、基础原理

### 1.1 什么是 Midjourney 视频？

Midjourney 于 2025 年 6 月正式推出其首个视频工作流——**Image-to-Video 模型**（内部代号 V1 Video Model）。该功能允许用户将任何 Midjourney 生成的静态图像（或上传的图像）转化为动态视频片段。与传统逐帧动画不同，Midjourney 的扩散视频模型**同时生成所有帧**，将它们视为一个统一的时空数据体（spatial-temporal volume），同时处理空间内容和时间序列 [1]。

这一并行处理方式意味着：提示词中的所有信息会被同时考虑，而非按时间线逐步执行。因此，撰写视频提示词时应当为整个视频序列提供完整的上下文描述。

### 1.2 技术规格

| 规格项 | 详情 |
|--------|------|
| 默认时长 | 5 秒（约 120-150 帧） |
| 帧率 | 24 fps（社交媒体优化）/ 30 fps |
| 最大时长 | 约 21 秒（通过 4 次延伸，每次约 4 秒） |
| 分辨率 | SD 480p（默认）/ HD 720p（高级计划可用） |
| 编码格式 | H.264 MPEG-4 AVC，Planar 4:2:0 YUV |
| GPU 消耗 | 约 8 倍普通图像生成（5 秒视频 ≈ 8 GPU 分钟） |
| 访问方式 | 仅通过 midjourney.com 网页端 |
| 计划支持 | Fast Mode 全计划可用；Relax Mode 仅 Pro/Mega |
| 提示词限制 | 总长度 < 220 字符（Discord 硬限制） |
| 批量生成 | 默认 4 个视频，可选 1 或 2 个以节省 GPU |

### 1.3 V8/V8.1 与视频功能的协同

V8 Alpha（2026 年 3 月发布）和 V8.1（2026 年 4 月发布）是**图像生成模型**的重大升级，而视频功能使用独立的 V1 Video Model。两者的协同关系体现在以下方面：

V8/V8.1 为视频功能带来的间接提升包括：原生 2K HD 渲染使得起始帧质量大幅提高，3 倍速度提升加快了起始帧的迭代效率，Style Creator 功能可以创建自定义风格并通过 `--sref` 在视频中保持一致，而 Draft Mode（64 张低分辨率缩略图批量生成）则让用户能够快速筛选最佳起始帧。

关键帧功能（Keyframes）是视频模型的重要更新：用户现在可以同时添加首帧和末帧，模型会在两帧之间生成过渡动画，这极大地增强了对视频叙事的控制力 [2]。

---

## 二、参数字典

### 2.1 视频专用参数

#### `--video` / `--video 1`

这是启动视频生成的核心参数。当用户上传图片 URL 并附加提示词时，添加此参数即可告知 Midjourney 生成视频而非图像。需要注意的是，`--video 1` 当前仅支持 480p 输出，高分辨率图片会被自动降采样 [3]。

#### `--motion low` 与 `--motion high`

`--motion` 参数控制动画的运动强度，是视频生成中最关键的控制杆之一。

`--motion low`（默认值）产生**温和、稳定的动画**，适合环境氛围场景、产品展示和需要保持画面完整性的场景。典型效果包括轻柔的漂移、微风吹拂、缓慢的镜头推进等。

`--motion high` 增加动画的强度和范围，适合**动态、高能量的场景**。它会放大镜头运动和角色动作，但也可能引入视觉不一致或伪影。建议在使用 high motion 时搭配更精确的提示词来约束运动方向 [4]。

#### `--raw`

`--raw` 标志禁用 Midjourney 的部分默认风格化解释，让**提示词对视频行为拥有更直接的控制权**。这在以下场景中特别有用：需要保留参考图片的原始结构、避免抽象视觉修饰、或者当自动辅助产生的结果不符合预期时。官方 FAQ 指南建议将 `--raw` 作为故障排除的首选步骤 [5]。

#### `--loop`

使用起始帧作为结束帧，创建**无缝循环视频**。这对于制作社交媒体背景、音乐可视化器和环境氛围视频非常有价值。配合 circular、rotating、orbital 等运动关键词效果最佳 [6]。

#### `--end <image_URL>`

允许用户指定一张不同的图片作为视频的**结束帧**，模型会在起始帧和结束帧之间生成过渡动画。这为场景转换、变形效果和叙事性视频提供了强大的控制手段。

#### `--bs <1|2|4>`

批量大小参数，控制每个提示词生成的视频数量。SD 模式下，1 个视频约 2 GPU 分钟，2 个约 4 分钟，4 个约 8 分钟。在探索阶段建议使用 `--bs 1` 节省成本，确认方向后再批量生成 [3]。

### 2.2 通用参数（同时适用于视频）

以下图像生成参数在视频模式下同样有效，可以与视频专用参数组合使用：

| 参数 | 功能 | 视频中的作用 |
|------|------|------------|
| `--ar <ratio>` | 宽高比 | 由起始帧决定视频尺寸，建议提前设置 |
| `--seed <number>` | 固定噪声模式 | 提高可复现性，便于 A/B 测试 |
| `--stylize / --s` | 风格化强度 | 控制 MJ 风格应用的程度 |
| `--chaos` | 变化/随机性 | 增加运动的多样性 |
| `--quality / --q` | 速度与细节权衡 | 影响视频的细节丰富度 |
| `--sref <URL>` | 风格参考 | 在视频中保持与参考图一致的视觉风格 |
| `--cref <URL>` | 人物参考 | 在视频中保持角色外观一致性 |
| `--stop <value>` | 提前停止生成 | 控制生成完成度 |

### 2.3 `--sref` 和 `--cref` 在视频中的一致性控制

`--sref`（Style Reference）允许用户提供一张风格参考图，Midjourney 会在视频生成过程中保持与该参考图一致的色彩、光影和整体美学风格。配合 V8 的 Style Creator 功能，用户可以创建高度定制化的风格并在多个视频中复用 [7]。

`--cref`（Character Reference）则专注于角色一致性。通过提供角色参考图，模型能够在视频的不同帧中保持角色的面部特征、服装和整体外观的一致性。这对于制作系列视频内容或角色驱动的叙事尤为重要。

---

## 三、实战案例

### 3.1 标准化工作流：从静态图到动态视频

**五步标准流程**如下 [3]：

**第一步：生成或上传起始帧。** 使用 `/imagine` 生成图像，或直接上传已有图片。起始帧的选择至关重要——它决定了风格、运动和内容的基调。建议使用网格图（grid image）而非放大图（upscale），以减少降采样伪影。

**第二步：选择动画模式。** 在 midjourney.com 网页端点击图片查看，选择 "Animate Manually"（手动模式）进入提示词编写界面。Auto 模式适合快速体验，但 Manual 模式才是精准控制的核心。

**第三步：编写运动提示词。** 删除原始图片提示词（它不是好的运动提示词），从零开始叙述视频片段。聚焦于运动、序列和物体持久性，而非静态视觉描述。

**第四步：选择运动强度。** 根据场景需求选择 `--motion low`（默认，适合安静场景）或 `--motion high`（适合动态场景）。可选添加 `--raw` 以获得更精确的提示词控制。

**第五步：延伸与导出。** 满意后可延伸视频（最多 4 次，每次约 4 秒），最终导出为社交媒体优化 MP4、原始 MP4 或 GIF 格式。

### 3.2 物理运动诱导策略

通过精心设计的提示词关键词，可以"诱导"特定类型的物理运动。以下是经过社区验证的运动诱导策略 [1] [4]：

#### 水流与液体

水流效果是 Midjourney 视频模型最擅长的物理模拟之一。有效关键词包括 `drifting through currents`、`waves crash`、`ripples spread`、`water cascading`。配合 `--motion low` 可获得优雅的慢速水流，`--motion high` 则产生激烈的浪花飞溅效果。

实战提示词示例：
> `a glowing jellyfish drifting through deep ocean currents, cinematic lighting, slow motion --motion low --video 1`

#### 火焰与烟雾

火焰和烟雾需要更强的运动强度来表现其动态特性。关键词如 `flames flickering`、`smoke curling upward`、`embers drifting`、`fire spreading` 能有效触发这类运动。建议搭配 `--motion high` 使用。

实战提示词示例：
> `phoenix reborn from ashes, reverse time burst, high motion sparks --video --motion high`

#### 面部表情与微表情

面部表情是最具挑战性的运动类型之一。模型对细微的面部变化有一定理解能力，但需要非常具体的描述。有效策略包括：描述表情变化的过程而非结果（如 "jaw tightening for a moment before relaxing"），使用情感副词（如 "slowly blinks, exhales with controlled tension"）。

实战提示词示例：
> `The camera begins with a slow, intimate dolly-in toward the man's face, his eyes glisten faintly, jaw tightening for a moment before relaxing into pained composure --motion low --raw`

#### 风与自然力

风的效果通过描述其对物体的影响来实现，而非直接描述风本身。有效关键词包括 `hair fluttering with rhythm`、`flags swaying in breeze`、`leaves peel off into wind`、`dust particles drift`。

#### "即将发生"（About To）技巧

这是社区发现的最有效的运动诱导技巧之一 [1]。核心思路是：创建动作**即将发生前一刻**的起始帧，而非动作进行中或之后的画面。例如，生成"狗即将咬汉堡"的图片，在动画时狗会自然地完成吃的动作。如果不在视频提示词中明确写出动作指令，主体可能会"拒绝"执行预期动作。

### 3.3 镜头语言控制

镜头运动是视频提示词中最强大的控制维度之一。以下是经过验证的镜头术语及其效果 [4] [5]：

| 镜头术语 | 效果描述 | 可靠性 |
|----------|---------|--------|
| `orbit` | 环绕主体旋转 | 高 |
| `dolly-in / push-in` | 向主体推进 | 高 |
| `dolly-zoom` | 希区柯克变焦效果 | 中 |
| `tilt-up / tilt-down` | 镜头上下倾斜 | 高 |
| `pan left / pan right` | 水平摇镜 | 高 |
| `crane / jib` | 起重机式升降 | 中 |
| `tracking / follow` | 跟踪拍摄 | 中 |
| `zoom in / zoom out` | 变焦推拉 | 高 |
| `handheld` | 手持晃动感 | 中 |
| `steady-cam / slider` | 稳定器滑轨感 | 中 |
| `POV / first-person` | 第一人称视角 | 低-中 |
| `rack focus` | 焦点转移 | 低 |

> **重要提示**：镜头术语有效但不总是可靠。更有效的方式是**描述运动的效果**而非使用技术术语。例如，"The camera slowly rises" 比 "crane shot" 更可靠。建议组合使用：术语 + 效果描述 [5]。

### 3.4 循环视频制作技巧

制作完美的无缝循环视频需要特殊的提示词策略 [6]：

使用 `--loop` 参数是基础，但还需要在提示词中配合循环友好的运动描述。有效关键词包括 `endless`、`seamless`、`radial motion`、`circular`、`rotating`、`orbital`、`continuous`、`infinite`、`repeating`。应避免有明确起止点的线性运动，转而使用环形或周期性运动。

### 3.5 隐藏低分辨率的构图策略

由于当前视频输出仅为 480p，聪明的构图可以有效掩盖分辨率限制 [6]：

**微距镜头**（Macro shots）通过极端特写隐藏像素密度问题。**剪影**（Silhouettes）利用高对比度减少对细节的依赖。**大气效果**如雾、烟、粒子可以增加感知质量。**高对比度和戏剧性光影**也能有效掩盖分辨率不足。

---

## 四、流派工作流

### 4.1 产品广告流派

产品广告是 Midjourney 视频最成熟的应用场景之一。核心工作流如下：

首先使用 V8/V8.1 生成高质量产品图（利用 Draft Mode 快速筛选最佳角度），然后在视频提示词中使用 `rotating on mirrored plinth`、`slow orbit cam`、`studio rim light` 等关键词。建议使用 `--motion low` 保持产品细节清晰，搭配 `macro push-in` 镜头隐藏 480p 分辨率。

典型提示词：
> `matte-black smart watch rotating on mirrored plinth, studio rim light, soft fog --ar 16:9 --video --motion low`

### 4.2 奇幻电影流派

奇幻场景允许更大的运动自由度和视觉创意。核心策略是利用 `--motion high` 配合戏剧性的镜头运动和粒子效果。

典型提示词：
> `crystal dragon bursts from glacier, aerial rise, snow shards glitter --ar 21:9 --video --motion high`

### 4.3 社交媒体循环流派

针对 Instagram Reels、TikTok 和 Twitter/X 的短循环视频。关键是选择正确的宽高比（9:16 竖屏 / 1:1 方形）和完美的循环效果。

| 平台 | 推荐宽高比 | 最佳时长 | 内容策略 |
|------|-----------|---------|---------|
| Instagram Reels | 9:16 | 3-5 秒循环 | 高对比度，视觉冲击 |
| TikTok | 9:16 或 1:1 | 5 秒循环 | 表情包风格，突然重置 |
| YouTube Shorts | 9:16 | 15-21 秒 | 教程风格，清晰进展 |
| Twitter/X | 1:1 或 16:9 | 5 秒完美循环 | 艺术/美学内容 |

### 4.4 叙事延伸流派

通过多次延伸构建完整的微叙事。核心技巧 [1]：

第一步，生成 5 秒基础片段。第二步，使用 Extend Manual 添加新动作（如 "cat leaps → cat lands"）。第三步，重复最多 4 次达到 21 秒的微型广告。每次延伸从上一个视频的最后一帧开始，模型会尝试理解前一段视频的上下文。

需要注意的是，视频质量随每次延伸递减，第三次延伸后尤为明显。建议将最重要的内容放在前 5 秒，并限制延伸次数在 2 次以内 [1]。

### 4.5 角色驱动流派

利用 `--cref` 和 `--sref` 在多个视频片段中保持角色和风格一致性。工作流包括：使用 Omni Reference 建立角色库，为角色选择标志性视觉属性（如独特的蓝眼睛），在每个场景中使用相同的 `--sref` 和 `--cref` 参数，为每个场景单独生成起始帧而非依赖延伸 [1]。

---

## 五、局限与避坑指南

### 5.1 已知局限

| 局限 | 详细说明 | 应对策略 |
|------|---------|---------|
| 仅支持图生视频 | 无法直接文字生成视频，必须先有起始帧 | 先用 V8 快速生成起始帧 |
| 分辨率有限 | SD 480p 为主，HD 720p 需高级计划 | 使用微距、剪影等构图策略 |
| 时长限制 | 最长约 21 秒 | 规划好每段的内容分配 |
| 物理交互不可靠 | 波浪、风力、碰撞等物理效果不稳定 | 尝试向模型"解释"物理过程 |
| 复杂旋转困难 | 详细主体的复杂旋转/转向效果差 | 尚无可靠解决方案 |
| 新元素引入困难 | 起始帧中不存在的元素难以可靠引入 | 在延伸阶段引入，或预先在起始帧中包含 |
| 无音频支持 | 不支持配音、脚本或文字叠加 | 后期在剪辑软件中添加 |

### 5.2 常见错误与避坑

**错误一：使用图片提示词作为视频提示词。** 图片提示词描述的是静态视觉，而视频提示词需要描述运动和序列。务必删除原始图片提示词，从零开始编写运动描述 [5]。

**错误二：在 5 秒内塞入过多动作。** Midjourney 不会加速动作来适应时间限制。动作需要比想象中更多的铺垫时间才能显得自然。建议每 5 秒只安排一个主要动作 [5]。

**错误三：描述首帧之前的动作。** 模型无法想象时间零点之前的事件。如果想要"镜头横扫丛林揭示老虎"，起始帧必须是丛林，提示词描述镜头从丛林移向老虎 [5]。

**错误四：镜头环绕与物体旋转混淆。** "Rotate around the statue" 和 "the statue rotates" 对语言模型来说几乎是同义词。需要明确描述镜头行为："The camera swings around the lizard in orbit. The background sweeps by." [5]

**错误五：使用放大图作为起始帧。** 放大图在降采样到 480p 时更容易产生伪影。建议使用网格图或经过 Creative Upscale 平滑处理的图片 [5]。

---

## 六、成本与效率优化

### 6.1 GPU 预算管理

| 操作 | GPU 消耗 | 估算成本（Pro 计划） |
|------|---------|-------------------|
| 5 秒视频（SD） | 约 8 GPU 分钟 | 约 $0.10 |
| 5 秒视频（HD） | 约 16 GPU 分钟 | 约 $0.20 |
| 4 秒延伸 | 约 6 GPU 分钟 | 约 $0.08 |
| 21 秒完整视频 | 约 32 GPU 分钟 | 约 $0.42 |

### 6.2 效率优化建议

在 Pro 计划下使用 Relax Mode 进行批量夜间生成，可以大幅降低成本。先使用 `--bs 1` 进行方向探索，确认后再用 `--bs 4` 批量生成。在动画前先放大图片（U1-U4）可以获得更清晰的纹理。使用 V8 的 Draft Mode 快速筛选最佳起始帧，避免在不理想的起始帧上浪费视频生成的 GPU 时间。

---

## 参考来源

[1] Geeky Curiosity, "How to write Midjourney video prompts that actually work", Substack, 2025  
[2] Rory Flynn, "Midjourney Video Keyframes released", LinkedIn, 2025  
[3] Midjourney Official Documentation, "Video", docs.midjourney.com  
[4] D-Libro, "Prompting motion for video control", d-libro.com  
[5] Midjourney FAQs, "Video Prompting Guide", Notion, 2026  
[6] SuperDuperAI, "Midjourney Motion Complete Guide", superduperai.co  
[7] bok-ai-lab, "Midjourney Tips for Videos", HackMD, 2025  
