# video-skill · 产品 AI 视频 workflow

雷迪恩（RAYDIENE）家用充电桩的 AI 视频片段生产 skill。把一份视频文案，变成可以直接
喂给图像模型和视频模型的 prompt 与镜头表。

**与 `xhs-skill` 完全无关。** 那个做小红书静态图文，这个做视频，不要混用。

## 链路

```
需求确认单 ─→ 镜头表 ─→ 图像 prompt（首帧/尾帧）─→ 图像 ─→ 视频 prompt ─→ 视频
                          └────── Claude 做到这里 ──────┘
                          └──────────── Codex 做到出图 ────────────┘
```

模型和版本由**操作者自行准备**。会过期的事实（能力边界、时长上下限、计费）全部集中在
`references/model-routing.md` 一份文件里，对不上就改那一份，其余 references 写的是不随版本变的方法论。

## 核心概念：首尾帧

生成的两张图不是「两张好看的图」，是**一段视频的两个端点**。

- **首帧** = 现象**开始之前**的定格（不是进行中的截图，这是最常见的错误）
- **尾帧** = 现象**结束之后**的第一刻，由首帧 prompt 复制后只改变化的那几句
- **过程** 由视频 prompt 承载，图不管过程

需要收敛（产品必须稳定、有明确状态变化、要卡点）走首尾帧；需要发散（氛围空镜、纯粒子光效）只出首帧。

## 目录

```
video-skill/
├── AGENTS.md                    Codex 入口
├── CHANGELOG.md                 倒序，接手先读
└── product-video/
    ├── SKILL.md                 调度：铁律、流程、闸门、references 索引
    ├── references/
    │   ├── shot-planning.md     需求确认单 + 文案拆镜 + 镜头表 schema
    │   ├── frame-strategy.md    首/尾帧语义、判据、增量规则、运镜归属
    │   ├── image-prompt.md      十三要素 + 四条画风轨道 + 产品参考句
    │   ├── video-prompt.md      过程描述写法
    │   ├── physical-realism.md  充电桩场景穿帮清单
    │   ├── model-routing.md     Kling/Seedance 能力边界与分流（会过期，只改这份）
    │   └── codex-execution.md   Codex 出图顺序与命名
    └── templates/
        └── shotlist-template.md
```

## 安装

```powershell
New-Item -ItemType SymbolicLink -Path "$env:USERPROFILE\.claude\skills\product-video" -Target "E:\Raydiene\app\video-skill\product-video"
```

Codex 侧把 `product-video/SKILL.md` 作为系统提示喂入。

## 两条最容易踩的

1. **首帧写成了过程中。** 「雨水冲刷着桩体」是错的，「桩体干燥，第一滴雨刚落在顶部」才对
2. **视频 prompt 里重复描述外观。** 会让视频模型重新想象产品，导致 logo 和配色在片中漂移
3. **按台词切出 1–2 秒的分镜然后一镜一生成。** 生成下限是 4 秒，短镜必须合并或走多镜头一次生成
4. **把社区 skill 的自定约定当成平台限制。** 踩过一次：社区 skill 弃用首尾帧 ≠ 平台不支持。以官方手册为准
