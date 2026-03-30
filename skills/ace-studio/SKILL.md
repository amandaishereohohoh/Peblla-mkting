---
name: ace-studio
description: Ace 剪辑师增强工具包 — Python 实现的视频合成/调色/动效引擎，模拟 DaVinci Resolve + Natron 核心功能
---

# Ace Studio

Ace 剪辑师的 Python 视频渲染工具包，提供合成、调色、动效、渲染四大引擎，
模拟 DaVinci Resolve 和 Natron/Fusion 的核心工作流。

## 能力概览

| 模块 | 能力 | 模拟对标 |
|------|------|----------|
| **合成引擎** | 图层叠加（alpha/screen/multiply）、矩形/圆形/羽化遮罩、色键抠像、运动追踪 | Natron / Fusion |
| **调色引擎** | Lift/Gamma/Gain 三路调色、LUT、HSL、色温/色调、S 曲线、暗角、Bloom | DaVinci Resolve Color |
| **动效引擎** | 关键帧动画（位置/缩放/旋转/透明度）、6 种缓动函数、文字动效、转场、粒子 | After Effects |
| **渲染管线** | 序列帧 PNG、ffmpeg H.264/H.265 编码、音频混合、分辨率预设、Letterbox | DaVinci Deliver |

## 依赖

```
pip install pillow numpy scipy
```

外部命令：`ffmpeg`（用于视频编码和音频混合）

## 使用方式

Ace 角色在制作分镜视频、拼贴镜头、添加动效时调用此工具包。
通过 `Pipeline` API 串联所有操作，最终 `render()` 输出视频文件。

### 快速开始

```python
import sys
sys.path.insert(0, ".claude/skills/ace-studio")
from ace_studio import Pipeline, ColorGrader, TextAnimation

p = Pipeline(width=1920, height=1080, fps=30, duration=5.0)

# 添加背景图片（持续全程）
p.add_clip("bg.png", start=0, end=5.0)

# 叠加产品图（带缩放动画）
p.add_clip("product.png", start=0.5, end=4.5,
           position=(960, 540), scale_keys={0.5: 0.0, 1.0: 1.0, 4.0: 1.0, 4.5: 0.0},
           easing="ease-out-back")

# 添加标题文字（slide-up 动效）
p.add_text("Peblla POS", start=1.0, end=4.0,
           position=(960, 200), font_size=72, color="#FFFFFF",
           animation="slide-up", anim_duration=0.6)

# 全局调色（暖琥珀 LUT）
p.set_color_grade(lut="warm-amber", vignette=0.3)

# 渲染输出
p.render("output.mp4", codec="h264", crf=18)
```

### Before/After 分屏对比视频示例

```python
from ace_studio import Pipeline

p = Pipeline(width=1920, height=1080, fps=30, duration=6.0)

# 左半屏：Before（原始画面）
p.add_clip("before.png", start=0, end=6.0,
           crop_rect=(0, 0, 960, 1080), position=(480, 540))

# 右半屏：After（调色后画面）
p.add_clip("after.png", start=0, end=6.0,
           crop_rect=(0, 0, 960, 1080), position=(1440, 540))

# 中间分割线（从上滑入）
p.add_clip("divider.png", start=0.3, end=5.7,
           position=(960, 540),
           position_keys={0.3: (960, -50), 0.8: (960, 540)},
           easing="ease-out")

# Before / After 标签
p.add_text("BEFORE", start=0.8, end=5.5,
           position=(480, 100), font_size=48, color="#FF3B30",
           animation="fade-in", anim_duration=0.4)

p.add_text("AFTER", start=0.8, end=5.5,
           position=(1440, 100), font_size=48, color="#34C759",
           animation="fade-in", anim_duration=0.4)

# 电影宽银幕黑边
p.set_letterbox(ratio=2.39)

p.render("before_after.mp4", codec="h264", crf=18)
```

### 调色示例

```python
from ace_studio import ColorGrader
from PIL import Image
import numpy as np

img = Image.open("raw_frame.png")
grader = ColorGrader()

# DaVinci 风格三路调色
grader.set_lift(r=-0.05, g=-0.02, b=0.03)    # 暗部偏冷
grader.set_gamma(r=1.0, g=0.98, b=0.95)       # 中间调微暖
grader.set_gain(r=1.05, g=1.0, b=0.95)        # 高光暖色

# 加 S 曲线提升对比度
grader.set_s_curve(shadows=-10, highlights=15)

# 暗角
grader.set_vignette(strength=0.4, radius=0.8)

result = grader.apply(img)
result.save("graded_frame.png")
```

## 能力边界

- **可以做**：单个分镜头渲染、拼贴两个镜头加画面动效和文字动效、简单 AE 特效、调色、粒子
- **不能做**：完整视频的全流程剪辑（多机位剪辑、复杂音频混音、字幕轨道管理），这些需要人工配合
- **定位**：类似 AI 视频生成软件 — 可以救急、可以做简单单镜头，但不替代专业 NLE

## 文件结构

```
.claude/skills/ace-studio/
  SKILL.md          # 本文件（Skill 描述）
  ace_studio.py     # 核心 Python 工具包（合成/调色/动效/渲染）
```
