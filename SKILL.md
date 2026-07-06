---
name: obsidian-ai-kb
description: Organize AI-related URLs (GitHub repos, articles, tweets, papers, tutorials, product reviews) into a structured Obsidian knowledge base at D:\data\AI. Use when the user gives one or more URLs about AI and wants them summarized and filed as notes. Dynamically routes content to existing sections by content type, and auto-creates new top-level sections or subcategories when content does not fit any existing one. Handles project notes, opinion/insight notes, tutorial notes, paper notes, and product review notes, each with a fixed frontmatter template, MOC updates, and source archival. Also use when the user asks to view, restructure, or maintain this knowledge base.
---

# Obsidian AI 知识库整理

把用户给的 AI 相关 URL 整理成结构化笔记，写入 Obsidian 知识库。

## 知识库根目录

`D:\data\AI`（即 Obsidian vault 根目录）。所有笔记写在此目录下。

## 顶层结构（动态生长，勿预建空分区）

```
D:\data\AI\
├── 00-首页\AI知识库首页.md          # 顶层导航 MOC（始终轻量，只放分区总链接）
├── 01-提示词\                       # 提示词模板（<10 篇内联首页；≥10 篇才拆 MOC）
├── 02-AI项目大全\                   # 开源项目（含子分类，必然有独立 MOC）
│   ├── _AI项目大全MOC.md
│   └── <子分类>\<项目名>.md
├── 03-行业观点与洞察\               # 人物观点/圆桌/推文（≥10 篇时拆独立 MOC）
│   └── _行业观点与洞察MOC.md         # 规模达标后才有，小则不建
├── ...（按内容自动扩展）
├── attachments\                     # 附件
└── .obsidian\                       # 配置（勿动）
```

约定：附件存 attachments\（.obsidian\app.json 已配置 ./attachments）；笔记用 UTF-8；链接用 Obsidian wiki link [[文件名]]（不含扩展名）。MOC 拆分时机见 3.5。

## 工作流程

### 1. 识别内容类型

根据 URL 与抓取后的内容形态判断（URL 来源只是线索，最终以内容形态为准）。

### 2. 抓取内容

- **GitHub 仓库**：调用 GitHub MCP get_repo（取元数据）+ fetch_file（取 README.md；中文项目可另取 README.zh-CN.md）。子目录 URL 先定位到 owner/repo 再抓根 README。一次抓不全（截断）时，用 fetch_file 的 start_line/end_line 分段或直接抓特定文件补全。
- **文章**：用 Invoke-WebRequest 抓 HTML 存到 attachments/<source>_<id>.html，再用 PowerShell 正则提取正文（常见容器：article-content / post_content / paragraph）。抓取需 sandbox_permissions: require_escalated。
- **X 推文**：x.com 页面 SSR 会截断正文，改用 https://api.fxtwitter.com/<user>/status/<id> 取完整 JSON，tweet.text 即正文；同时抓原页面 HTML 存档。
- **论文 PDF**：优先抓 abstract/简介页；若需全文可用 Invoke-WebRequest 下载 PDF 到 attachments/ 再提取。

### 3. 分类决策与归类（关键环节）

先扫描已有分区，再按内容形态匹配；不命中则自动新建。**不要预建空分区，只在有真实内容时才创建。**

#### 3.1 扫描已有分区

- 顶层分区：扫描 D:\data\AI 下形如 NN-名称 的目录（NN 为两位数字前缀）。
- 项目子分类：扫描 02-AI项目大全 下的子目录（若 02 存在）。

#### 3.2 按内容形态匹配

| 内容形态 | 去向（已存在时） | 不存在时 |
| --- | --- | --- |
| GitHub 仓库 / 开源项目 | 02-AI项目大全\<子分类> | 新建子分类（见 3.3） |
| 人物观点、圆桌讨论、推文观点 | 03-行业观点与洞察 | 已为核心分区，通常已存在 |
| 教程、课程笔记、学习路径 | 04-学习与教程 | 新建顶层分区 |
| 论文阅读、技术报告、白皮书 | 05-论文与研报 | 新建顶层分区 |
| 产品测评、使用心得、产品对比 | 06-AI产品体验 | 新建顶层分区 |
| 行业数据、benchmark、市场报告 | 07-数据与基准 | 新建顶层分区 |

#### 3.3 新建分区规则

**顶层分区**：
- 编号：取已有最大编号 +1（核心分区预留 00-08；新分类从 04 起；超过 09 顺延 10、11...）。
- 命名：NN-中文名（如 04-学习与教程）。
- 命中「主题明确可独立成类」才建；主题模糊或仅单条难归类时不建，改归 09-暂存与待分类（首次需要时才创建）。

**项目子分类**（02-AI项目大全 下）：
- 文件夹名用中文主题词（如 AI Agent框架、LLM评估与测试）。
- 不命中且主题明确时新建子分类文件夹。
- 同步在 _AI项目大全MOC.md 加分类小节。

#### 3.4 新建顶层分区后必须

1. 创建目录；
2. 更新 00-首页\AI知识库首页.md：在「分区导航」加小节（编号 · 名称 + 一句说明）；目录结构示意按实际扫描结果重写（不要保留过期的固定文本）。

#### 3.5 独立 MOC 的拆分时机（关键规则）

**是否给一个分区建独立 MOC，不取决于内容类型，而取决于规模与结构。**

- **默认**：所有分区先内联在首页「分区导航」下（逐条列出笔记链接）。
- **拆出独立 MOC 的触发条件**（满足其一即可）：
  1. 该分区笔记数 ≥ 10 篇；
  2. 该分区出现子分类/层级（如 02-AI项目大全 的子文件夹）。
- **拆分时**：
  1. 在分区目录下建 `_XX MOC.md`（如 `_行业观点与洞察MOC.md`）；
  2. 首页对应小节从「逐条链接」改为「一行总链接」：`- [[_XX MOC]] — 一句说明（含 N 篇 / M 个分类）`；
  3. MOC 内部可按主题/时间再分小节，承担原本堆在首页的细粒度索引。
- **已拆 MOC 的分区新增笔记时**：只更新该分区 MOC（加链接行 + 刷新统计），不再动首页；首页仅在统计数字变化时偶发刷新。
- **不要**为只有几篇笔记的分区预建 MOC（过度设计）；也**不要**在分区已超阈值后仍把全部链接塞在首页（首页臃肿）。

> 首页的定位是「顶层轻量导航」：永远只放分区总链接，不堆细粒度条目。重索引下沉到各分区 MOC。

### 4. 写笔记（用模板）

模板见 references/templates.md。笔记类型按内容形态选：project-note / opinion-note / tweet-note / tutorial-note / paper-note / review-note（后三类复用 opinion-note 结构，调整 frontmatter 字段与正文章节）。

所有笔记统一要素：
- YAML frontmatter（字段见模板）
- 开头 > [!info] 一句话定位/观点 callout
- 观点类必须含 > [!warning] 时效性提示 或等价说明
- 末尾「在生态中的定位」表（项目）或「我的解读/待跟踪」（观点/其他）
- 尽量用 [[...]] 链接到已有笔记，形成知识网络

### 5. 更新索引

每次新增后必须更新（用 Set-Content 整体重写，保证幂等）。**索引落点取决于该分区是否已拆独立 MOC（见 3.5），而非内容类型：**

- **未拆 MOC 的分区**（笔记 < 10 且无子分类）：00-首页\AI知识库首页.md — 在对应分区小节下加一行 `- [[笔记名]] — 一句话简介`。
- **已拆 MOC 的分区**（≥10 篇或有子分类）：该分区的 `_XX MOC.md` — 在对应小节下加一行链接；更新 统计数字 / 最近更新。首页不动（除非要刷新统计行）。
  - **02-AI项目大全** 恒为已拆 MOC（必有子分类）：更新 _AI项目大全MOC.md（分类下加行 + 项目总数/分类数/最近更新）。
  - **其他分区**：按 3.5 阈值判断。达阈值时先拆 MOC（建文件 + 首页小节改为总链接），再加行。
- **新建分区**：按 3.4 处理首页导航与目录结构示意。
- **暂存区**：09-暂存与待分类 的条目也加到首页导航末尾，标注「待归类」，便于定期清理。

### 6. 验证

用 Get-ChildItem -Recurse 列出受影响目录的文件，确认笔记就位、MOC 链接文件名与实际文件名一致。

## 注意事项

- 抓取网络内容需在 shell_command 上加 sandbox_permissions: require_escalated 并写 justification。
- 中文写入用 PowerShell here-string 配合 Set-Content -Encoding UTF8，避免 BOM 与编码问题。
- 不要 git commit 或建分支，除非用户明确要求。
- 笔记要精炼，避免大段照搬 README；提炼「解决什么问题 + 怎么用 + 在生态中位置」。
- 观点笔记务必保留信源 URL 与日期；推文易失效，须把完整原文贴进笔记末尾，并把 HTML/JSON 存到 attachments/。
- 多个项目可并行抓取（GitHub MCP 调用无依赖），但写文件与更新 MOC 要串行避免冲突。
- 分区按需创建，宁缺毋滥：单条内容优先归已有分区或暂存，不要为一篇笔记新建一个分区。