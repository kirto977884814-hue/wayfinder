# wayfinder

> Claude skills that help you find your way — not tell you where to go.

**wayfinder** 是一套协作的 Claude Code 技能，构成一个「个人发展引擎」：从你是谁，推导你可能成为什么，再到怎么去。

它的角色是**人生地图师**——不是导师、不是测评师、不是推荐官。不替你决定人生，只帮你逐渐看见：自己在哪、世界有多大、下一步可以往哪里走。

## 三个技能

| 技能 | 干什么 |
|------|--------|
| **selfport** | 播客式长期访谈，构建可持续更新的人生画像 |
| **exploration** | 读画像，推导你可能感兴趣的方向 + 卡在哪，用轻量任务驱动你迈出去 |
| **teach** | 接过你选的方向，生成学习 mission，排互动课程 |

一句话：**selfport 告诉你「你是谁」，exploration 告诉你「你可能成为什么」，teach 告诉你「怎么去」。**

## 数据怎么流转

三个技能通过本地数据契约协作，数据全程在本地：

```
selfport  画像  (~/.personal-profile/PROFILE/)
              ↓  只读
exploration  抽象 Interest DNA → 推方向 + 一个推荐 + 深入三档
              ↓  handoff：方向 + 痛点
teach       生成 MISSION.md → 排 HTML 课程
```

## 哲学

三者共享同一套姿态：

- **不装懂** —— 知识来自高信任资源，不凭记忆瞎编；画像永远不完整，所以不替你下判断。
- **基于真实数据** —— 一切判断挂回你的画像和真实反馈，不写通用清单。
- **不替你决定** —— 给方向、给依据，让你自己选。

## 安装

把 `skills/` 下的三个目录复制进 Claude Code 技能目录：

```bash
# Unix / macOS
cp -r skills/* ~/.claude/skills/

# Windows (Git Bash)
cp -r skills/* "/c/Users/<你>/.claude/skills/"
```

然后：`/selfport` 建画像 → `/exploration` 探索 → `/teach` 学习。

## 隐私

- **你的画像和学习数据永远在本地**（`~/.personal-profile/`），不在本仓库、不上传。
- 本仓库只含技能代码（SKILL.md、引导脚本、模板骨架）。
- 技能间协作的数据契约见 [selfport/DATA_CONTRACT.md](skills/selfport/DATA_CONTRACT.md)。

## 状态

`exploration` 是本套体系里新设计的；`selfport` 与 `teach` 已稳定可用。仍在迭代。

## License

MIT
