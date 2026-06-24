# MiMo-Code 视频多模态支持改造

> 在小米 MiMo-Code（OpenCode fork）上添加原生视频文件输入支持。

## 改动内容

| 文件 | 改动说明 |
|------|---------|
| `packages/opencode/src/util/media.ts` | 新增 `isVideoAttachment()` + 视频 magic bytes 检测 |
| `packages/opencode/src/tool/read.ts` | `read` 工具识别视频，转 base64 附件返回（50MB 上限） |

> 注：协议层（`openai-chat.ts`）在 MiMo-Code 中是 npm 依赖 `@opencode-ai/llm`，不在本地源码中。如需完整链路，需要 patch 该 npm 包或使用 opencode 仓库的完整改造版。

## 详细文档

完整的设计思路、架构分析、使用方法见配套仓库：[SunNull/opencode/VIDEO-SUPPORT.md](https://github.com/SunNull/opencode/blob/main/VIDEO-SUPPORT.md)

## 上游

- [XiaomiMiMo/MiMo-Code](https://github.com/XiaomiMiMo/MiMo-Code) - 原始仓库
