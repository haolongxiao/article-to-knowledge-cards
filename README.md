# make-knowledge-cards

Trae Skill：把文章和本地 Markdown / TXT 文件转换成 5~8 张高质量知识卡片。

---

## 这个 Skill 解决什么问题

读完一篇好文章，常常觉得"好像懂了，但记不住"。考试前翻笔记，翻到的是密密麻麻的段落，抓不住重点。知识卡片（Knowledge Cards）把文章拆成一张张独立的、结构化的小单元，每张只讲一个知识点，让记忆和复习变得高效。

## 主要功能

- **智能提取知识点** — 自动识别文章的核心概念、定义、对比和流程，跳过寒暄、广告、重复段落
- **一卡一知识点** — 每张卡片只讲一件事，互不重复，独立可读
- **结构化卡片格式** — 每张卡片包含：**标题 · 核心知识（一句话 TL;DR）· 简明解释 · 例子或自测题**
- **严格不编造** — 所有内容必须来自原文，不会凭空补充事实或数据
- **数量自适应** — 短文（200 字以下）2~3 张，中等长度 4~6 张，长文 6~8 张，不硬凑
- **同语言输出** — 中文文章出中文卡片，英文文章出英文卡片

### 输入支持

| 类型 | 说明 |
|------|------|
| 对话粘贴文本 | 直接把文章贴到 Trae 对话里 |
| 本地 `.md` 文件 | 提供文件路径 |
| 本地 `.txt` 文件 | 提供文件路径 |

### 不支持

网页 URL 抓取、PDF 文件、Anki 导入格式、图形界面

---

## 安装方法

### 方式一：项目级安装（推荐）

把 `skills/make-knowledge-cards/` 整个文件夹放到你的 Trae 项目根目录下即可：

```
your-project/
├── skills/
│   └── make-knowledge-cards/
│       ├── SKILL.md
│       └── agents/
│           └── openai.yaml
```

Trae 会自动识别项目下的 Skill，在该项目中即可生效。

### 方式二：全局安装（跨项目可用）

把 `skills/make-knowledge-cards/` 整个文件夹拷贝到 Trae 的全局 Skill 目录：

- Windows：`%USERPROFILE%\.trae\skills\make-knowledge-cards\`
- macOS / Linux：`~/.trae/skills/make-knowledge-cards/`

安装后，所有 Trae 项目都可以触发这个 Skill。

### 验证安装

在 Trae 对话中输入 `#`，在 Skill 列表中应该能看到 **Make Knowledge Cards**。

---

## 使用方法

### 方式一：自然语言触发（最简单）

直接在 Trae 对话中描述需求，Skill 会自动匹配触发：

```
帮我把这篇文章转换成知识卡片：
[粘贴文章内容]
```

或者指定本地文件：

```
请把 ./articles/restful-api.md 转成知识卡片
```

### 方式二：显式调用

在输入框中输入 `#`，从弹出的 Skill 列表中选择 `make-knowledge-cards`，然后附上文章内容或文件路径。

---

## 输入输出示例

### 输入（一段短文）

```markdown
## 什么是 RESTful API

REST 是 Representational State Transfer 的缩写，由 Roy Fielding 在 2000 年的博士论文中提出。它不是一种协议，而是一组设计 Web 应用程序接口的架构风格和约束条件。

RESTful API 的核心原则之一是无状态通信：每次请求必须包含所有必要的信息，服务器不保存客户端的会话状态。这意味着服务器可以轻松水平扩展。

REST 使用 HTTP 协议的标准方法来表示不同的操作。GET 用于获取资源，POST 用于创建新资源，PUT 用于整体更新，PATCH 用于部分更新，DELETE 用于删除资源。
```

### 输出（4 张知识卡片）

```markdown
### 📌 卡片 1 — REST 是什么

| 字段 | 内容 |
|------|------|
| **核心知识** | REST 是一种设计 Web API 的架构风格，不是协议。 |
| **简明解释** | REST 全称 Representational State Transfer，由 Roy Fielding 在 2000 年博士论文中提出。它是一组设计约束，遵循这些约束的 API 就叫 RESTful API。 |
| **例子 / 自测** | 判断题：REST 定义了严格的通信协议。（答案：✗，REST 是架构风格，不是协议） |

### 📌 卡片 2 — 无状态通信的含义

| 字段 | 内容 |
|------|------|
| **核心知识** | 每次 HTTP 请求必须自包含，服务器不保存会话状态。 |
| **简明解释** | 服务器不记得"上一次请求发生了什么"，每个请求都必须带齐所有必要信息。好处是服务器可以轻松水平扩展，因为每个请求都是独立处理的。 |
| **例子 / 自测** | 为什么无状态有利于服务器扩展？（答案：任何一台服务器都能处理任何请求，不需要会话亲和） |

### 📌 卡片 3 — HTTP 方法映射 CRUD 操作

| 字段 | 内容 |
|------|------|
| **核心知识** | REST 用 HTTP 标准方法表示 CRUD 操作。 |
| **简明解释** | GET 查、POST 建、PUT 整体改、PATCH 部分改、DELETE 删。每个方法有明确的语义，客户端一看就知道要做什么。 |
| **例子 / 自测** | 用哪个方法部分更新一个用户的 email 字段？（答案：PATCH，PUT 通常用于整体替换） |

### 📌 卡片 4 — URI 标识资源的原则

| 字段 | 内容 |
|------|------|
| **核心知识** | REST 用名词复数形式的 URI 标识资源，不用动词。 |
| **简明解释** | /users/123 表示 ID 为 123 的用户资源。用 HTTP 方法表达动作（GET、POST...），不要把动作塞进 URI（如 /getUser）。 |
| **例子 / 自测** | 以下哪个 URI 更符合 REST 风格？A) /deleteUser?id=42  B) DELETE /users/42 （答案：B） |
```

---

## 卡片质量保证

| 规则 | 说明 |
|------|------|
| 一张卡片一个知识点 | 不把多个概念塞进同一张卡 |
| 只提取重要内容 | 跳过寒暄、广告、重复段落 |
| 严格不编造 | 所有信息必须来自原文 |
| 删除重复 | 同一知识点只讲一次 |
| 标题具体 | 拒绝"原则一"、"基本概念"这种笼统标题 |
| 例子 / 自测二选一 | 原文有好例子就用例子，没有就出自测题 |

---

## 项目结构

```
.
├── README.md
├── quick_validate.py
└── skills/
    └── make-knowledge-cards/
        ├── SKILL.md
        └── agents/
            └── openai.yaml
```

- `SKILL.md` — Skill 的核心指令文档（frontmatter + 使用说明）
- `agents/openai.yaml` — Trae UI 展示用的元数据（display_name、short_description 等）
- `quick_validate.py` — 官方 Skill 结构验证脚本

### 运行验证

```bash
pip install pyyaml
python quick_validate.py skills/make-knowledge-cards
# 输出: Skill is valid!
```

---

## License

MIT
