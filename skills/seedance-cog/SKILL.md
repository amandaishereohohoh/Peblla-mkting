# Seedance × CellCog — 全流程视频制作方法论

**Seedance × CellCog.** ByteDance 的 #1 视频模型 + 多智能体协调框架。

Seedance 生成最流畅的 AI 视频——1080p 电影级画面，物理效果逼真。CellCog 在此基础上协调脚本、语音合成、口型同步、配乐和剪辑，从单一 prompt 生成完整视频。不只是片段——而是完整作品。

---

## 视频制作全流程 Pipeline

```
脚本撰写 → 场景规划 → 关键帧生成 → 语音合成
 → 口型同步 → 背景音乐 → 音效设计 → 剪辑 → 最终输出
```

**6-7 个基础模型**协同工作：
- Seedance：视频生成
- 前沿 LLM：脚本撰写
- TTS 模型：语音合成
- Lipsync 模型：口型对齐
- 音乐生成：配乐
- 自动剪辑：节奏与转场

---

## 可创作的视频类型

### 营销视频 (Marketing Videos)
- **产品演示**："Create a 60-second product demo video for our project management app"
- **品牌故事**："Create a 30-second brand story video for a sustainable fashion startup"
- **社媒广告**："Create a 15-second Instagram ad for our new coffee blend"
- **用户证言**："Create a UGC-style testimonial video for a fitness product"

### 解说视频 (Explainer Videos)
- **产品解说**："Create a 90-second explainer for how our API works"
- **概念科普**："Create a video explaining blockchain in simple terms"
- **教程视频**："Create a step-by-step tutorial video on setting up a home network"

### 电影级内容 (Cinematic Content)
- **短片**："Create a 2-minute short film about a robot discovering nature"
- **MV**："Create a cinematic music video with dramatic landscapes"
- **品牌影片**："Create a cinematic brand film for a luxury watch company"

### 真人演讲视频 (Spokesperson Videos)
- **新闻播报**："Create a news-style report on recent AI developments"
- **培训视频**："Create a training video with a presenter explaining safety protocols"
- **产品发布**："Create a product launch announcement with a spokesperson"

---

## 视频规格

| 规格 | 详情 |
|------|------|
| **分辨率** | 最高 1080p |
| **时长** | 3 秒 ~ 4 分钟 |
| **风格** | 写实、电影、动漫、风格化、纪录片 |
| **音频** | 语音合成、背景音乐、音效 |
| **输出** | MP4 |

---

## 写出更好视频 Prompt 的 5 个原则

1. **讲故事而非描述功能**："A video about our app" → "A 60-second video showing a stressed founder discovering our app, their workflow transforming, ending with them confidently presenting to investors"

2. **明确时长**："30-second social ad" vs. "2-minute explainer"——时长决定节奏

3. **设定情绪基调**："Upbeat and energetic"、"calm and professional"、"dramatic and cinematic"

4. **指定音乐偏好**："Uplifting corporate background"、"lo-fi beats"、"cinematic orchestral"——或让系统自动选择

5. **真人演讲视频要描述形象**：描述演讲者的外貌特征和声音风格

---

## 核心理念（全团队内化）

这套方法论是视频制作团队每位成员的**职业素养**：
- **Rex（导演）**：用 Pipeline 思维分配任务，确保每个环节衔接流畅
- **Sage（编剧）**：按"讲故事"原则写脚本，为后续 AI 生成提供精准指引
- **Coco（视觉设计师）**：掌握视频规格和 Prompt 原则，确保关键帧提示词精准有效
- **Ace（剪辑师）**：理解全流程 Pipeline，在剪辑环节把控节奏、转场和音效
