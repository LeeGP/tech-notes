# Tech Notes

个人技术博客 demo：用 **Astro + Markdown** 记录技术方案与学习笔记。前期本地运行、几乎零成本；文章以 Markdown 存放，后续可迁到其他框架或免费静态托管。

## 环境要求

- Node.js 18+（推荐 20 或 22）
- npm 9+

## 本地启动

```bash
cd F:\NewIdea\tech-notes
npm install
npm run dev
```

浏览器打开终端提示的本地地址（默认 `http://localhost:4321`）。

## 构建

```bash
npm run build
npm run preview
```

静态产物输出到 `dist/`，可直接部署到任意静态托管。

## 如何新增文章

1. 在 `src/content/blog/` 新建 `.md` 文件
2. 填写 frontmatter：

```yaml
---
title: 标题
description: 摘要
pubDate: 2026-07-21
tags:
  - 技术方案
draft: false
---
```

3. 编写正文后，用 `npm run dev` 预览

## 后期免费部署（可选）

把仓库推到 GitHub 后，可在 **Cloudflare Pages** 或 **GitHub Pages** 连接该仓库：构建命令 `npm run build`，输出目录 `dist`。绑定自定义域名也是之后再做即可。
