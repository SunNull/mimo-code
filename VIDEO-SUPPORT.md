# MiMo-Code 视频多模态支持改造

> 在小米 MiMo-Code（OpenCode fork）上添加原生视频/音频文件输入支持。

## 改动内容（读取层，已同步）

| 文件 | 改动说明 |
|------|---------|
| `packages/opencode/src/util/media.ts` | 新增 `isVideoAttachment()` / `isAudioAttachment()` + 视频/音频 magic bytes 检测 |
| `packages/opencode/src/tool/read.ts` | `read` 工具识别视频/音频，转 base64 附件返回（50MB 上限） |
| `packages/opencode/src/tool/read.txt` | 工具描述加入视频/音频说明 |

## 架构说明（更正）

MiMo-Code 是较老的 OpenCode fork，**没有独立 native runtime / `@opencode-ai/llm` 包**。它的协议层是**本地 vendored 的 AI SDK 转换器**：

- `packages/opencode/src/provider/sdk/copilot/chat/convert-to-openai-compatible-chat-messages.ts:46` —— 只处理 `image/`，视频/音频会落到 `UnsupportedFunctionalityError`。
- 配套类型：`packages/opencode/src/provider/sdk/copilot/chat/openai-compatible-api-types.ts`（仅 `image_url`）。

也就是说：**读取层现在能识别视频/音频并生成附件，但发送层（AI SDK 路径）仍会拒绝非图片内容。** 要打通完整链路，必须改这个 vendored 转换器，加入 `video_url` / `input_audio` 分支。

## 剩余阻塞（按「AI SDK 先不动」策略暂缓）

| 待办 | 位置 | 说明 |
|------|------|------|
| 发送层支持 `video_url` | `convert-to-openai-compatible-chat-messages.ts` + `openai-compatible-api-types.ts` | 属 AI SDK 路径改造，当前策略暂缓 |
| 发送层支持 `input_audio` | 同上 | MiMo 音频入参：`{ type: "input_audio", input_audio: { data: "data:audio/xxx;base64,..." } }` |

> 如需立即启用完整视频/音频链路，使用 **opencode 仓库**的 native runtime 路径（见下方链接），它已端到端打通且无需改 AI SDK。

## 详细文档

完整设计思路、架构分析、使用方法、音频/压缩等进一步改造见配套仓库：
[SunNull/opencode/VIDEO-SUPPORT.md](https://github.com/SunNull/opencode/blob/main/VIDEO-SUPPORT.md)

## 上游

- [XiaomiMiMo/MiMo-Code](https://github.com/XiaomiMiMo/MiMo-Code) - 原始仓库
