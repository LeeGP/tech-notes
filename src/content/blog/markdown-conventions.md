---
title: Markdown 写作约定与标签实践
description: 本站文章 frontmatter 字段说明，以及标签如何用于筛选。
pubDate: 2026-07-20
tags:
  - 学习笔记
  - Markdown
  - 技术方案
draft: false
---

本站每篇文章都是 `src/content/blog/` 下的一个 `.md` 文件。

## Frontmatter

```yaml
---
title: 文章标题
description: 一句话摘要
pubDate: 2026-07-20
tags:
  - 技术方案
  - Markdown
draft: false
---
```

## 标签怎么用

- 首页与文章页的标签可点击，进入 `/tags/<tag>/`
- 建议标签表达「主题」，例如：`Astro`、`学习笔记`、`技术方案`
- `draft: true` 的文章不会出现在首页列表（构建仍会生成页面，发布前请谨慎）

## 新增文章步骤

1. 复制任意一篇样例 Markdown
2. 改文件名与 frontmatter
3. 本地运行 `npm run dev` 预览

写完即可提交到 Git；内容始终是普通文本，迁移成本最低。
