# frag-knowledge

Claude Code Skill — 碎片知识捕获、翻阅与审阅。零外部依赖。

核心理念：**AI 追问，人来总结。** AI 不替你想，而是帮你想到位。

## 三个流程

| 流程 | 触发词 | 做什么 |
|------|--------|--------|
| 捕获 | "记一下xxx" | 原样保存 + 苏格拉底追问 + 共同反思 |
| 翻阅 | "翻一翻" | FSRS 调度选卡 + 评分（忘了/模糊/记得住/很熟） |
| 审阅 | "帮我审一下" | 扫描缺反思/过期笔记 + 回炉对话 |

附加功能："做成卡片" → 按 Wozniak 20 Rules 生成 Anki 卡片。

## 三层架构

```
Level 0: Claude Code + 本地目录          ← 当前版本
Level 1: + Obsidian vault 写入
Level 2: + Anki/AnkiConnect 制卡同步
```

**Level 0（已实现）**：纯 Claude Code，不需要 Obsidian、Anki 或任何 API。笔记存储在 `~/.frag-knowledge/`。

**Level 1（规划中）**：检测 Obsidian vault，笔记直接写入 `{vault}/frag-knowledge/`，复用 wikilinks 和 graph view。

**Level 2（规划中）**：检测 AnkiConnect，在线时直接添加卡片到 Anki deck，不在线则保存到 `pending-cards.json` 等待同步。

### 产品边界

**我们做的：** 碎片捕获 + AI 追问反思（核心价值）、自动归档、轻量翻阅

**不做的（交给已有工具）：** 卡片复习 → Anki、知识库管理 → Obsidian、同步 → Obsidian Sync

## 安装

```bash
# 复制 SKILL.md 到 Claude Code skills 目录
mkdir -p ~/.claude/skills/frag-knowledge
cp SKILL.md ~/.claude/skills/frag-knowledge/
```

然后在 Claude Code 中使用：
```
/frag-knowledge 记一下：能量守恒定律和能源危机的关系
```

## 存储结构

```
~/.frag-knowledge/
  inbox/              ← 新捕获（draft）
  reviewing/          ← 正在讨论修改中的
  mature/             ← 归档的成熟笔记
    concepts/         ← 概念（方法论、原理）
    sources/          ← 来源（文章、演讲、论文）
    entities/         ← 实体（人、公司、工具）
  review-state.json   ← FSRS 调度状态
  pending-cards.json  ← 待同步到 Anki 的卡片
```

## 文件格式

```markdown
---
type: concept
date: 2026-07-02
tags: [capture]
quality: draft
---

# 标题

## 原始内容

用户提供的原文，一字不改。

## 反思

人和 AI 讨论后共同得出的反思。

## 追问记录

- Q: 追问
- A: 回答
```

## 配套：提示工程框架

`docs/prompt-framework/` 包含两套 AI 进课堂的思维镜头：

- **通用镜头**（6 个）：跨界迁移、时间穿越、极限条件、反转目标、拆解假设、跨感官
- **Nendo 镜头**（10 个）：轮廓、错误、过程、相乘、链接、隐藏、皮肤、平衡、折叠、放大

用途：用镜头激发学生的创新思维，与知识捕获流程配合使用。

## 已知问题

1. 自动触发不生效 — 需要显式 `/frag-knowledge` 调用
2. 制卡流程设计完成，未实际测试
3. review-state.json 首次翻阅时自动创建

## 参考

- [Deep Student](https://github.com/helixnow/deep-student) — ChatAnki 流程
- [Anki](https://github.com/ankitects/anki) — FSRS 算法
- [AnkiDroid](https://github.com/ankidroid/Anki-Android) — Android Anki

## License

MIT
