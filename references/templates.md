# 笔记模板

笔记类型按内容形态选择。前三种为核心模板，后三种复用 opinion-note 结构并调整字段。

## 1. project-note（GitHub 项目 / 开源项目）

~~~
---
title: <项目显示名>
repo: <owner/repo>
url: https://github.com/<owner/repo>
category: <子分类>
one_liner: <一句话定位，用于 MOC 索引行>
language: <Python / TypeScript / ...>
license: <MIT / Apache-2.0 / ...>
maintainer: <维护方>
docs: <文档/官网 URL>
created: <YYYY-MM-DD>
updated: <YYYY-MM-DD>
tags: [ai-project, <主题标签...>]
---

# <项目名> — <副标题>

> [!info] 一句话定位
> <一句话说清是什么、解决什么>

- 仓库地址：[<owner/repo>](<url>)
- 文档：<docs>
- 语言 / 许可：<language> / <license>
- 维护方：<maintainer>

## 核心作用
<解决什么问题、为什么需要它。2-3 段，含「典型场景」列表>

## 主要特性
- <emoji> **特性名**：<简述>（4-6 条，提炼 README 核心能力，不要照搬）

## 使用方式
~~~bash
# 安装
<命令>
# 基本用法
<命令或代码示例>
~~~
<必要的配置说明、注意事项>

## 在生态中的定位
| 维度 | 说明 |
| --- | --- |
| 类别 | <分类> |
| 关键词 | <3-5 个> |
| 同类对比 | <与已有项目的差异，用 [[...]] 链接> |
| 上手难度 | 低 / 中 / 中高 |
| 推荐指数 | <星> |

## 备注
<社区、衍生项目、安全提示等>
~~~

## 2. opinion-note（文章/圆桌观点）

~~~
---
title: <标题>
speaker: <发言人>（圆桌可列多位）
speaker_role: <职务>
source: <媒体/栏目>
source_url: <原文 URL>
source_date: <YYYY-MM-DD>
topic: <主题>
timeliness: 时效性观点（<日期>，<为何可能过期>）
created: <YYYY-MM-DD>
updated: <YYYY-MM-DD>
tags: [观点, 行业洞察, <主题标签...>]
---

# <标题>

> [!info] 一句话观点
> <核心论断>

## 人物与信源
- **观点提出者**：<人物>，<职务>
- **信源**：[<媒体> <日期>](<url>)

> [!warning] 时效性提示
> <日期>观点，<领域>变化快，引用请注意时间背景。

## 核心观点
<按人物或论点分块，每块小标题 + 2-4 句提炼>

## 我的解读 / 待跟踪
- <与已有笔记 [[...]] 的呼应>
- <可作自审框架/选型参考的部分>
- <待跟踪的开放问题>

## 相关报道（同信源提及）
- <同篇文章提到的其他相关观点>
~~~

## 3. tweet-note（推文观点）

~~~
---
title: <人物>：<主题>
speaker: <姓名>
speaker_handle: "@<handle>"
speaker_role: <身份>
source: X (Twitter)
source_url: https://x.com/<handle>/status/<id>
source_date: <YYYY-MM-DD>
topic: <主题>
timeliness: <方法论型 / 时效型>观点（<说明>）
engagement: <likes> likes / <reposts> reposts / <replies> replies
created: <YYYY-MM-DD>
updated: <YYYY-MM-DD>
tags: [观点, 行业洞察, <主题标签...>]
---

# <人物>：<主题>

> [!info] 一句话观点
> <核心论断>

## 人物与信源
- **作者**：<姓名>（[@<handle>](<url>)），<身份>
- **互动**：<赞 / 转 / 评>

> [!note] 时效性说明
> <方法论型：框架较稳定，解法会演进；时效型：随领域变化>

## 核心框架 / 观点
<推文如有结构化框架，用表格或列表呈现；否则分点提炼>

## 我的解读 / 待跟踪
- <与已有笔记的呼应，用 [[...]] 链接>

## 推文原文
> <把完整推文原文贴在这里。推文易被删除/不可访问，必须保留全文。>
~~~

## 4. tutorial-note（教程/课程笔记，归 04-学习与教程）

复用 opinion-note 结构，frontmatter 调整：
- 去掉 speaker/speaker_role，加 author（作者/讲师）、course（课程名，可选）。
- source 改为来源（博客/YouTube/Coursera 等）；timeliness 改为 content_type: 教程。
- 正文章节改为：学习目标 → 核心知识点（分块）→ 实操要点 → 我的练习/待深入。
- 不强制时效性警示（教程相对稳定），但技术栈版本敏感时加 note。

## 5. paper-note（论文/技术报告，归 05-论文与研报）

复用 opinion-note 结构，frontmatter 调整：
- 加 authors、venue（会议/期刊）、arxiv（arXiv ID）、year。
- source_url 指向 arXiv/论文页；timeliness 改为 content_type: 论文。
- 正文章节改为：研究问题 → 方法 → 关键结论 → 实验/数据 → 我的解读/可复现点。
- 关联：若论文对应有开源实现，用 [[项目名]] 链接到 02-AI项目大全。

## 6. review-note（产品测评，归 06-AI产品体验）

复用 opinion-note 结构，frontmatter 调整：
- 加 product（产品名）、vendor（厂商）、version（版本/日期）。
- source_url 指向测评原文；timeliness 改为 content_type: 产品测评。
- 正文章节改为：产品定位 → 核心能力 → 体验亮点与槽点 → 适用场景 → 与竞品对比（用 [[...]] 链接同类产品）。

## frontmatter 字段说明

- created/updated：YYYY-MM-DD。
- tags：第一个标签区分类型（ai-project / 观点 / 教程 / 论文 / 产品测评），后接主题标签。
- timeliness / content_type：标注内容性质，便于判断引用时是否需复核时效。
- MOC 索引行的简介取自 frontmatter 的 one_liner（项目）或标题（其他）。
