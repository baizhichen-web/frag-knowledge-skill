# frag-knowledge 发展路线

## 产品定位

碎片知识不是独立的复习系统或制卡工具。它是**知识捕获和反思的连接层**——在已有的 Anki、Obsidian 之间做桥接，不重复造轮子。

核心差异化：**AI 追问，人来总结。**

## 三层架构

```
Level 0: Claude Code + 本地目录（当前）
Level 1: + Obsidian vault 写入
Level 2: + Anki/AnkiConnect 制卡
```

### Level 0 — 零依赖（已实现）

纯 Claude Code 驱动，不需要任何外部工具。

- 存储：`~/.frag-knowledge/{inbox, reviewing, mature/}`
- 捕获："记一下xxx" → 原样保存 + 苏格拉底追问 + 共同反思
- 翻阅："翻一翻" → FSRS 调度选卡 + 评分
- 审阅："帮我审一下" → 扫描缺反思/过期笔记 + 回炉
- 制卡："做成卡片" → Wozniak 20 Rules 生成卡片 → pending-cards.json

**现状**：已测试捕获和审阅流程，翻阅流程设计完成待测试。

### Level 1 — Obsidian 集成

用户有 Obsidian，笔记直接写入 vault。

- 写入路径：`{vault}/frag-knowledge/inbox/`（环境变量 `OBSIDIAN_VAULT_PATH`）
- 复用 Obsidian 的 wikilinks、backlinks、graph view
- 归档时自动创建 Obsidian 兼容的 frontmatter（aliases, tags）
- 支持从 Obsidian 内直接查看和编辑笔记

**待做**：
- [ ] 检测 Obsidian vault 路径（自动或配置）
- [ ] 写入时生成 Obsidian 兼容 frontmatter
- [ ] 测试 Obsidian 打开后的渲染效果

### Level 2 — Anki 集成

用户有 Anki + AnkiConnect，卡片直接同步。

- 检测 AnkiConnect（`curl http://localhost:8765`）
- 在线 → 逐张添加到 deck "frag-knowledge"
- 不在线 → 保存到 `pending-cards.json`，下次检测时同步
- 卡片模板：问答题（正面/背面）

**待做**：
- [ ] 测试 AnkiConnect 实际调用
- [ ] 设计卡片模板（问答题/填空题/反向题）
- [ ] pending-cards.json 同步逻辑

## 产品边界

**我们做的：**
1. 碎片捕获 + AI 追问反思（核心独特价值）
2. 自动归档到 Obsidian Wiki 知识库
3. 轻量浏览和翻阅

**我们不做的（交给已有工具）：**
- 卡片复习 → Anki / AnkiDroid
- AI 制卡 → Deep Student
- 知识库管理 → Obsidian
- 同步 → Obsidian Sync

## 配套项目

### 提示工程框架

独立于 frag-knowledge 的设计创新工具，存放在 `docs/prompt-framework/`。

- **通用思维镜头**（6 个）：跨界迁移、时间穿越、极限条件、反转目标、拆解假设、跨感官
- **设计方法论镜头**（Nendo 10 个）：轮廓、错误、过程、相乘、链接、隐藏、皮肤、平衡、折叠、放大

用途：AI 进课堂时，用镜头激发学生的创新思维。与 frag-knowledge 的知识捕获流程配合使用。

**待扩展镜头**：
- [ ] Dieter Rams 十诫
- [ ] 深泽直人的"无意识设计"
- [ ] 原研哉的"空"
- [ ] Victor Papanek 的"为真实世界设计"
- [ ] Buckminster Fuller 的"协同效应"

## 已知问题

1. **自动触发不生效** — "记一下xxx" 不会自动触发 skill，需要显式 `/frag-knowledge` 调用（Claude Code skill 系统限制）
2. **`## 原始内容` 格式** — 已统一，但早期创建的文件可能格式不一致
3. **review-state.json** — 初始化为空结构，首次翻阅时自动创建

## 参考产品

- **Deep Student**（helixnow/deep-student）— Tauri 2, ChatAnki 流程, 模板系统
- **Anki**（ankitects/anki）— Rust 核心, FSRS 算法, AnkiConnect API
- **AnkiDroid** — Android 开源 Anki 客户端
