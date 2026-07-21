---
title: 用静态站点搭个人技术博客的选型思路
description: 为什么前期选 Astro + Markdown，以及如何为后期迁移保留空间。
pubDate: 2026-07-18
tags:
  - 技术方案
  - Astro
draft: false
---

个人技术博客前期最重要的不是功能堆叠，而是：**内容可带走、成本可控、架构可升级**。

## 推荐起点

- **内容**：仓库内的 Markdown（本项目就是）
- **渲染**：静态站点生成（SSG）
- **托管**：Cloudflare Pages / GitHub Pages 等免费静态托管

这样几乎零服务器账单，同时文章本身不锁死在某个 SaaS 上。

## 最小实现示意

```ts
// 文章模型保持简单，迁移时只搬文件与 frontmatter
type Post = {
  title: string;
  description: string;
  pubDate: Date;
  tags: string[];
  draft?: boolean;
};
```

## 后期怎么扩

1. 搜索：接入 Pagefind
2. 评论：Giscus（仍免费）
3. 需要动态能力时，再考虑边缘函数或独立后端

先把写作闭环跑通，比过早引入 CMS 更划算。
