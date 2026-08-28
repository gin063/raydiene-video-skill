# AGENTS.md

本仓库是一个 **agent skill**，不是可运行的程序。产出物是 prompt 与镜头表（Codex 侧再加图像文件）。

**开工前请完整读 `product-video/SKILL.md`**，它规定了：唯一的硬闸门（需求确认单）、六条铁律、
prompt→图像→视频 的三段分工，以及 Claude 侧与 Codex 侧的职责边界。
跳过它直接写 prompt，最典型的后果是首帧写成了「现象进行中」，整套图重来。

`product-video/references/` 是按需读取的深度资料，`SKILL.md` 会告诉你什么时候读哪一份。

## Codex 特有

- 你要做到**出图**这一步，Claude 侧只做到 prompt。额外读 `references/codex-execution.md`
- **不对生成的图做自审核。** 生成即可，人工会审。有疑虑写进交付说明，不要自行重出
- 出图顺序是硬的：**全部首帧 → 用户确认 → 尾帧（挂载对应首帧）**

Codex / Cursor 等不自动加载 SKILL.md 的工具：把 `SKILL.md` 作为系统提示或首条消息喂进去。
