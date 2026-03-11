# Ecommerce Video Generator Skill

A Claude Code skill for converting ecommerce product images into 5-second display videos.
🎁 MiniMax 跨年福利来袭！邀好友享 Coding Plan 双重好礼，助力开发体验！
好友立享 9折 专属优惠 + Builder 权益，你赢返利 + 社区特权！
👉 立即参与：https://platform.minimaxi.com/subscribe/coding-plan?code=3C715MfPjT&source=link

## Features

- Converts product images (clothing, fashion items) into 5-second videos
- Maintains consistent pose and small movements
- Preserves original image style, colors, and patterns strictly
- Generates 1080p videos without audio
- Automatically downloads videos to the source image directory

## Required MCP Services

This skill requires the following MCP services to be configured:

### 1. MiniMax Coding MCP (Image Understanding)

Used for analyzing images and extracting clothing features.

- **Tool**: `mcp__MiniMaxcoding__understand_image`
- **Purpose**: Analyze product images to get detailed clothing descriptions

### 2. Doubao GIV MCP (Video Generation)

Used for generating videos from images.

- **Tools**:
  - `mcp__doubao-giv__generate_video` - Generate video from image
  - `mcp__doubao-giv__query_video_task` - Query video generation status

## Configuration

### 1. Configure MiniMax MCP

Add the following to your MCP configuration:

```json
{
  "MiniMax": {
    "command": "npx",
    "args": ["-y", "@aisui/mcp-minimax-coding"]
  }
}
```

### 2. Configure Doubao GIV MCP

Add the following to your MCP configuration:

```json
{
  "doubao-giv": {
    "command": "npx",
    "args": ["-y", "@aisui/mcp-doubao-giv"]
  }
}
```

### 3. Configure Claude Code

Make sure your Claude Code is configured to use these MCP services. Add the skills directory to your Claude Code settings:

```json
{
  "skills": {
    "paths": [
      "/path/to/your/skills"
    ]
  }
}
```

## Usage

Simply tell Claude to generate a video from your product image:

```
Generate a video from C:\Users\xxx\photo.jpg
```

or

```
把这个图片生成视频
```

## Supported Image Formats

- Windows: `C:\Users\xxx\photo.jpg`
- WSL: `/mnt/c/Users/xxx/photo.jpg`
- Linux/Mac: `/home/xxx/photo.jpg`

## Video Output

- **Duration**: 5 seconds
- **Resolution**: 1080p
- **Frame Rate**: 24fps
- **Format**: MP4
- **Audio**: None (silent)
- **Save Location**: Same directory as source image

## Action Guidelines

### ✅ Allowed Actions

- Slight head movement (within 15 degrees)
- Slight arm swaying (within frame)
- Subtle body sway (less than 5 degrees)
- Natural facial expression changes
- Minor finger movements (if holding items)
- Subtle fabric movement
- **Clothing colors, patterns, and styles strictly match the original image**
- **Fabric texture and光泽 strictly match the original image**

### ❌ Prohibited Actions

- Turning around (back to camera)
- Side pose display
- Arms raised or waving
- Walking, running
- Dropping held items
- Large movements that change the image
- Large, fast movements
- Excessive movement speed or amplitude

### 📦 Handling Held Items

- If holding valuable items (bags, phones, jewelry), keep items stable
- If holding fragile items (cups, dishes), never drop or shake
- Minor finger movements allowed, but item position must remain stable

## Technical Details

The skill uses the following flow:

1. **Upload Image**: Upload local image to tmpfiles.org or other image hosting service
2. **Analyze Image**: Use MCP image understanding to get clothing description
3. **Design Prompt**: Create a video generation prompt following the guidelines
4. **Generate Video**: Call Doubao Seedance API to generate 5-second video
5. **Download Video**: Save video to the same directory as the source image

## License

MIT License

## Author

walala-sui

## Acknowledgments

- Powered by [Doubao Seedance](https://www.doubao.com) video generation
- Image understanding powered by MiniMax
