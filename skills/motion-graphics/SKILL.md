---
name: motion-graphics
description: 平面 MG 动效视频制作。当用户要求制作 logo 动画、品牌动效、片头片尾、文字动效、icon 动画、MG 动画、motion graphics、CSS 动画视频、逐帧渲染视频时使用。支持 Python+ffmpeg 导出 MP4 和 HTML+CSS+SVG 网页预览两种路径。
---

# Motion Graphics — 平面 MG 动效制作

Ace 的核心动效制作能力，支持两种输出路径：Python 逐帧渲染导出 MP4，或 HTML/CSS/SVG 网页动画。

## 动效方法论（刘紫涵训练·永久）

### 五大原则

1. **信息层级决定动画节奏**：主体先动 → 附属后动 → 装饰最后动。icon 是视觉主角必须先完成入场站稳，文字才跟上。
2. **三幕结构**：进场（20-25%）→ 展开（~27%）→ 定格（~47%）。电影叙事节奏压缩到 5 秒。
3. **Stagger 错开时序**：多元素不同时出现，每隔固定帧数依次弹入，让视线有引导方向。
4. **物理因果性**：每个运动有"来处"和"去处"，不凭空出现或消失。
5. **克制原则**：高级感来自减法。白色背景、无粒子特效、无渐变光晕、无多余装饰。

### 时间轴设计模板（5s @ 30fps = 150帧）

```
f0-6:    白屏留白（呼吸空间）
f6-36:   主体入场（icon/logo，选择入场方式）
f36-38:  主体脉冲（scale 1.0→1.06→1.0，微弹性）
f38-60:  主体移动到最终位置（ease_in_out_cubic）
f44-69:  文字/附属元素 stagger 弹入（每隔5帧，ease_out_back）
f78+:    微呼吸浮动（1px, 5s 周期正弦波）
f140-150: 尾部微暗收束
```

## 缓动曲线库

### Python 实现

```python
def clamp(v, lo=0.0, hi=1.0):
    return max(lo, min(hi, v))

def ease_out_cubic(t):
    return 1 - (1 - t) ** 3

def ease_out_back(t, s=1.3):
    t1 = t - 1
    return 1 + t1 * t1 * ((s + 1) * t1 + s)

def ease_out_quart(t):
    return 1 - (1 - t) ** 4

def ease_in_out_cubic(t):
    return 4 * t * t * t if t < 0.5 else 1 - (-2 * t + 2) ** 3 / 2

def ease_out_expo(t):
    return 1.0 if t >= 1.0 else 1 - 2 ** (-10 * t)

def ease_in_quad(t):
    return t * t

def progress(frame, start, end):
    """时间轴控制：返回 frame 在 [start, end] 区间内的进度 0-1"""
    if frame < start: return 0.0
    if frame >= end: return 1.0
    return (frame - start) / (end - start)
```

### Python → CSS 映射

| Python 缓动 | CSS cubic-bezier | 用途 |
|-------------|-----------------|------|
| ease_out_back(t, 1.3) | `cubic-bezier(0.34, 1.56, 0.64, 1)` | 弹性落地，入场/弹入 |
| ease_out_cubic(t) | `cubic-bezier(0.33, 0, 0.2, 1)` | 柔和减速，渐显/渐隐 |
| ease_in_out_cubic(t) | `cubic-bezier(0.65, 0, 0.35, 1)` | 丝滑位移，A→B 移动 |
| ease_out_expo(t) | `cubic-bezier(0.16, 1, 0.3, 1)` | 极速启动长尾，页面切换 |
| ease_in_quad(t) | `cubic-bezier(0.55, 0, 1, 0.45)` | 慢启快退，退场 |

### 核心原则

- **入场用 ease-out**（快→慢）
- **退场用 ease-in**（慢→快）
- **位移用 ease-in-out**（慢→快→慢）
- **弹性加 back/spring**
- **永远不用 linear**

## 路径 A：Python + Pillow + ffmpeg（导出视频）

### 素材准备

1. **SVG 矢量提取**：从网站 HTML 源码提取 `<path>` 数据，或从 Figma 导出
2. **Puppeteer 4x 渲染**：用 Node.js + Puppeteer 将 SVG 渲染为高清透明 PNG
3. **按元素拆分**：icon、每个字母、UI 模块各自独立文件

Puppeteer 渲染模板：
```javascript
const puppeteer = require('puppeteer');
const SCALE = 4; // 4x for quality

async function renderSVG(html, width, height, outputPath) {
  const browser = await puppeteer.launch({ headless: 'new', args: ['--no-sandbox'] });
  const page = await browser.newPage();
  await page.setViewport({ width, height, deviceScaleFactor: SCALE });
  await page.setContent(html, { waitUntil: 'networkidle0' });
  await page.screenshot({ path: outputPath, omitBackground: true });
  await browser.close();
}
```

### 渲染引擎

```python
from PIL import Image, ImageDraw
import subprocess, tempfile, os

W, H = 1920, 1080
FPS = 30
TOTAL = int(FPS * 5.0)
FFMPEG = "/Users/gezenghui/feishu-claude-bot/node_modules/ffmpeg-static/ffmpeg"

# 逐帧渲染
tmp_dir = tempfile.mkdtemp()
for i in range(TOTAL):
    img = Image.new('RGBA', (W, H), (255, 255, 255, 255))
    # ... 计算动画状态，绘制元素 ...
    img.convert('RGB').save(os.path.join(tmp_dir, f"f_{i:04d}.png"))

# ffmpeg 编码
cmd = [
    FFMPEG, '-y',
    '-framerate', str(FPS),
    '-i', os.path.join(tmp_dir, 'f_%04d.png'),
    '-c:v', 'libx264', '-preset', 'slow', '-crf', '17',
    '-pix_fmt', 'yuv420p', '-movflags', '+faststart',
    output_path
]
subprocess.run(cmd)
```

**ffmpeg 参数说明**：
- `-crf 17`：视觉无损（越低越清晰，18-23 常用）
- `-preset slow`：慢编码换更好压缩率
- `yuv420p`：所有播放器兼容
- `+faststart`：moov atom 前移，网页秒开

### Alpha 工具函数

```python
def apply_icon_alpha(icon_img, alpha):
    """给 RGBA 图像应用全局透明度"""
    if alpha >= 255: return icon_img
    r, g, b, a = icon_img.split()
    a = a.point(lambda x: int(x * alpha / 255))
    return Image.merge('RGBA', (r, g, b, a))

def resize_smooth(img, tw, th):
    return img.resize((max(1, tw), max(1, th)), Image.LANCZOS)
```

## 路径 B：HTML + CSS + SVG（网页预览）

适合快速预览、交互展示、不需要导出视频文件的场景。

### 结构模板

```html
<!DOCTYPE html>
<html>
<head>
<style>
  body { width: 1920px; height: 1080px; background: #fff; overflow: hidden;
         display: flex; align-items: center; justify-content: center; }

  .icon { opacity: 0;
          animation: fadeIn 0.8s cubic-bezier(0.33, 0, 0.2, 1) 0.2s forwards; }

  .letter { opacity: 0; transform: translateY(40px); }
  .letter-1 { animation: letterIn 0.53s cubic-bezier(0.34, 1.56, 0.64, 1) 1.47s forwards; }
  .letter-2 { animation: letterIn 0.53s cubic-bezier(0.34, 1.56, 0.64, 1) 1.63s forwards; }
  /* ... stagger: animation-delay 递增 ~170ms ... */

  @keyframes fadeIn { 0% { opacity: 0; } 100% { opacity: 1; } }
  @keyframes letterIn {
    0%   { opacity: 0; transform: translateY(40px); }
    100% { opacity: 1; transform: translateY(0); }
  }

  /* 微呼吸 */
  .logo { animation: breathe 5s ease-in-out 2.6s infinite; }
  @keyframes breathe {
    0%, 100% { transform: translateY(0); }
    50%      { transform: translateY(-1px); }
  }
</style>
</head>
<body>
  <div class="logo">
    <svg class="icon"><!-- icon path --></svg>
    <svg>
      <g class="letter letter-1"><!-- letter path --></g>
      <!-- ... -->
    </svg>
  </div>
</body>
</html>
```

### CSS Stagger 计算

```
base_delay = 1.47s  (= f44 / 30fps)
interval = 0.167s   (= 5帧 / 30fps)

letter-1: delay = 1.47s
letter-2: delay = 1.63s
letter-3: delay = 1.80s
letter-4: delay = 1.97s
letter-5: delay = 2.13s
letter-6: delay = 2.30s
```

## Icon 入场动画变体库

| 变体 | 名称 | 效果 | 适用场景 |
|------|------|------|---------|
| A | 积木拼合 | icon 拆成 5 碎片从四方飞入拼合 | 力量感、构建感 |
| B | 渐隐渐显 | fade in + 脉冲 | 优雅、简洁 |
| C | 粒子汇聚 | 小方块从四周汇聚成图标 | 聚合、凝聚力 |
| D | 笔触描绘 | 轮廓描线→填充→完整 | 手工感、匠心 |

选择原则：品牌调性越克制选 B，越有力量感选 A，越有科技感选 C。

## 参考实现

- **Python 4 变体渲染器**：`gen_outro_all.py`（当前 session 目录）
- **HTML/CSS 版本**：`peblla_outro_b.html`（当前 session 目录）
- **SVG 素材渲染器**：`render_logo_parts.cjs`（Puppeteer 4x 渲染）

## 工作流总结

```
需求 → 确定风格/时长/输出格式
        ↓
素材准备：SVG 提取 → Puppeteer 渲染 PNG / 直接内联 SVG
        ↓
选择路径：
  A) Python → 设计时间轴 → 逐帧渲染 → ffmpeg → MP4
  B) HTML  → CSS animation + SVG → 单文件 HTML
        ↓
预览 → 迭代调整缓动/时序/间距 → 交付
```
