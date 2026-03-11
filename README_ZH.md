# 电商图片转视频技能

一个用于将电商商品图片转换为5秒展示视频的Claude Code技能。
🎁 MiniMax 跨年福利来袭！邀好友享 Coding Plan 双重好礼，助力开发体验！
好友立享 9折 专属优惠 + Builder 权益，你赢返利 + 社区特权！
👉 立即参与：https://platform.minimaxi.com/subscribe/coding-plan?code=3C715MfPjT&source=link

## 功能特性

- 将商品图片（服装、时尚单品）转换为5秒展示视频
- 保持人物姿态自然，动作小幅度
- 严格保持与原图一致的服装款式、颜色、图案
- 生成1080p无声音视频
- 自动下载视频到原图片目录

## 所需MCP服务

此技能需要配置以下MCP服务：

### 1. MiniMax Coding MCP（图片识别）

用于分析图片并提取服装特征。

- **工具**：`mcp__MiniMaxcoding__understand_image`
- **用途**：分析商品图片获取详细的服装描述

### 2. Doubao GIV MCP（视频生成）

用于从图片生成视频。

- **工具**：
  - `mcp__doubao-giv__generate_video` - 从图片生成视频
  - `mcp__doubao-giv__query_video_task` - 查询视频生成状态

## 配置方法

### 1. 配置MiniMax MCP

在MCP配置中添加：
编辑配置文件 ~/.claude.json，添加以下 MCP 配置：
```json
{
  "mcpServers": {
    "MiniMax": {
      "command": "uvx",
      "args": ["minimax-coding-plan-mcp", "-y"],
      "env": {
        "MINIMAX_API_KEY": "MINIMAX_API_KEY",
        "MINIMAX_API_HOST": "https://api.minimaxi.com"
      }
    }
  }
}
```

### 2. 配置Doubao GIV MCP

在MCP配置中添加：

```json
"doubao-giv": {
      "command": "npx",
      "args": [
        "-y",
        "doubao-image-video-mcp@latest"
      ],
      "env": {
        "DOUBAO_API_KEY": "APIKEY",
        "DOUBAO_IMAGE_ENDPOINT_ID": "ep-xxxxxxxxxx",
        "DOUBAO_VIDEO_ENDPOINT_ID": "ep-xxxxxxxxx"
      }
    }
```

### 3. 配置Claude Code

确保Claude Code已配置使用这些MCP服务。在Claude Code设置中添加技能目录：

```json
{
  "skills": {
    "paths": [
      "/path/to/your/skills"
    ]
  }
}
```

## 使用方法

只需告诉Claude从商品图片生成视频：

```
用 C:\Users\xxx\photo.jpg 生成一个视频
```

或者

```
Generate a video from this image
```

## 支持的图片格式

- Windows: `C:\Users\xxx\photo.jpg`
- WSL: `/mnt/c/Users/xxx/photo.jpg`
- Linux/Mac: `/home/xxx/photo.jpg`

## 视频输出规格

- **时长**：5秒
- **分辨率**：1080p
- **帧率**：24fps
- **格式**：MP4
- **音频**：无（静音）
- **保存位置**：与源图片相同目录

## 动作规范

### ✅ 允许的动作

- 轻微的头部转动（不超过15度）
- 手臂轻微摆动（不离开画面）
- 身体轻微左右晃动（幅度小于5度）
- 面部表情自然变化
- 手指轻微动作（如果手上有物品）
- 衣角、头发等细节微动
- **服装颜色、图案、款式严格保持与原图完全一致**
- **面料、光泽、纹理严格保持与原图一致**

### ❌ 禁止的动作

- 转身（背对镜头）
- 侧身展示（侧面对镜头）
- 手臂高举挥舞
- 行走、跑动
- 丢弃手中物品
- 大幅度动作导致画面与原图不符
- 大幅度快速的动作
- 动作幅度过大或速度过快

### 📦 手持物品处理

- 如果人物手持贵重物品（包、手机、珠宝等），动作应保持物品稳定
- 如果人物手持易碎物品（杯子、碗碟等），绝对不能丢弃或摇晃
- 轻微的手指活动可以保留，但物品位置保持不变

## 技术细节

技能使用以下流程：

1. **上传图片**：将本地图片上传到tmpfiles.org或其他图床
2. **分析图片**：使用MCP图片识别获取服装描述
3. **设计提示词**：按照规范创建视频生成提示词
4. **生成视频**：调用豆包Seedance API生成5秒视频
5. **下载视频**：将视频保存到与源图片相同的目录

## 许可证

MIT License

## 作者

walala-sui

## 致谢

- 视频生成由 [豆包Seedance](https://www.doubao.com) 提供支持
- 图片识别由 MiniMax 提供支持
