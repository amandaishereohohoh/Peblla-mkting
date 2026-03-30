---
name: image-gen-api
description: 使用 Google Gemini API 生成或编辑图片（API 模式）。当用户要求生成图片、画图、创建图像、设计图片、编辑图片、修改图片、换背景、调整透视时触发。关键词：生图、画图、生成图片、画一个、设计一张、编辑图片、修图、换背景、image、generate image、AI画图。与 flyer-design-generation 区分：本 skill 使用 Google Gemini API 直接生成。
---

# Image Gen API (Google Gemini)

通过 Google Gemini API 生成或编辑图片，支持中英文 prompt。

## 环境变量

需要在 `session.env` 中配置以下环境变量（Claude 进程启动时自动加载）：

- `$GOOGLE_GEMINI_API_KEY` — Google Gemini API 密钥（用于图片生成和编辑）

脚本中通过 `os.environ.get("GOOGLE_GEMINI_API_KEY")` 读取，无需额外 source。

## 两种模式

### 模式 A：纯文本生图（无参考图）

用户只给文字描述，从零生成图片。

```bash
python3 .claude/skills/image-gen-api/scripts/gemini-gen.py "<prompt>" "<output_path>"
```

### 模式 B：图片编辑（用户发送了图片）

用户发了图片 + 编辑指令。系统会自动将图片保存到 session 目录，prompt 中会附带路径：
`[用户发送的图片已保存: /path/to/upload-xxxxx.jpg]`

使用编辑脚本，传入原图路径 + 编辑 prompt：

```bash
python3 .claude/skills/image-gen-api/scripts/gemini-edit.py "<input_image_path>" "<prompt>" "<output_path>"
```

## 使用流程

### 1. 判断模式
- 用户消息中有 `[用户发送的图片已保存: ...]` → 模式 B（图片编辑）
- 否则 → 模式 A（纯文本生图）

### 2. 优化 Prompt
将用户的描述翻译/优化为适合 AI 的英文 prompt（更详细、更具描述性）。

### 3. 调用对应脚本
- output_path 使用当前 session 目录下的文件，如 `./generated_<timestamp>.png`
- 脚本输出格式：`OK|<实际保存路径>|<字节数>` 表示成功
- 超时 180 秒

### 4. 查看并发送
用 Read 工具查看生成的图片，确认后将文件**绝对路径**包含在回复中（系统会自动通过飞书发送）。

### 5. 错误处理
- API 返回错误 → 告知用户具体原因
- 常见错误：配额不足、prompt 被安全过滤、网络超时

## 输出格式

回复时简短说明，附带图片文件路径：

```
已为你生成图片：xxx 🎨

/absolute/path/to/generated_xxx.png
```

## 注意事项

- 使用模型 `gemini-3.1-flash-image-preview`
- 支持 responseModalities: ["TEXT", "IMAGE"]
- Prompt 建议用英文以获得更好效果
- 图片编辑时，编辑指令要具体明确
