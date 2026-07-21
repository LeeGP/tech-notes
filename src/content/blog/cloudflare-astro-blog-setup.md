---
title: 从零搭建低成本技术博客：Astro + GitHub + Cloudflare 全链路
description: 记录本次个人博客从本地脚手架到免费上线的完整步骤，以及各软件在链路中的作用。
pubDate: 2026-07-21
tags:
  - 技术方案
  - Astro
  - Cloudflare
draft: false
---

本文记录把本站从零搭起来的过程：内容用 Markdown，站点用 Astro 静态生成，代码放 GitHub，托管在 Cloudflare，对外域名使用 `*.workers.dev`。

## 目标与结果

- **目标**：前期几乎零成本；文章可带走；以后能扩展（自定义域名、搜索、评论等）
- **线上地址**：`https://tech-notes.legp.workers.dev/`
- **本地路径**：`F:\NewIdea\tech-notes`

## 全链路一张图

```text
写 Markdown
    ↓
Astro 生成静态 HTML（本地可用 npm run dev / build）
    ↓
Git 记录版本
    ↓
推送到 GitHub 仓库
    ↓
Cloudflare 拉取代码并 npm run build
    ↓
部署到 workers.dev，浏览器访问
```

## 涉及软件 / 服务及用途

| 名称 | 用途 |
| --- | --- |
| **Cursor / 编辑器** | 写文章、改站点代码、与 AI 协作 |
| **Node.js + npm** | 运行 Astro；执行 `npm install` / `npm run dev` / `npm run build` |
| **Astro** | 把 `src/content/blog/*.md` 编译成静态网站 |
| **Git** | 本地版本管理（提交、历史、回滚） |
| **GitHub** | 远程代码托管；作为 Cloudflare 的构建来源 |
| **GitHub CLI (`gh`)** | 本机登录 GitHub、创建仓库、推送（可选，也可用 GitHub Desktop） |
| **Cloudflare Workers / Pages** | 免费构建、部署、HTTPS、提供 `*.workers.dev` 域名 |

说明：用 GitHub 登录 Cloudflare 时，账号可能带上 GitHub 绑定邮箱信息；`workers.dev` 中间那段子域名可在 Cloudflare 帐户里单独修改，不必删号重开。

## 详细步骤（本次实际做过的）

### 1. 创建项目目录

- 在 `F:\NewIdea` 下创建文件夹 `tech-notes`
- 用 Cursor 把工作区切到该目录

### 2. 初始化 Git

```bash
git init
```

配置本机署名（只需一次，用你的 GitHub 名和邮箱/noreply）：

```bash
git config --global user.name "你的名字"
git config --global user.email "你的邮箱或noreply"
```

**不要**把 GitHub 密码发给任何人；用浏览器 OAuth 登录即可。

### 3. 搭建 Astro 博客骨架

项目关键结构：

```text
tech-notes/
  src/
    content/blog/          # 文章 Markdown（核心资产）
    content.config.ts      # 文章字段校验
    layouts/BaseLayout.astro
    pages/index.astro      # 首页列表
    pages/blog/[...slug].astro
    pages/tags/[tag].astro
    styles/global.css
  templates/写作模板.md    # 本地写作模板（复制用）
  package.json
  astro.config.mjs
  README.md
```

依赖安装与本地命令：

```bash
npm install
npm run dev      # 本地预览，默认 http://localhost:4321
npm run build    # 产出 dist/
```

文章 frontmatter 约定：`title`、`description`、`pubDate`、`tags`、`draft`。

### 4. 登录 GitHub 并推送仓库

- 安装并登录 GitHub CLI：`gh auth login`（选 HTTPS + 浏览器登录）
- 创建公开仓库并推送（示例）：仓库名 `tech-notes`，地址形如 `https://github.com/LeeGP/tech-notes`

本地常用：

```bash
git add .
git commit -m "说明这次改了什么"
git push
```

### 5. Cloudflare 连接仓库并部署

1. 打开 [Cloudflare Dashboard](https://dash.cloudflare.com)
2. **Workers & Pages** → Create → 连接 GitHub 仓库 `LeeGP/tech-notes`
3. 构建设置：
   - Build command：`npm run build`
   - Output / 静态资源目录：`dist`（若向导是 Workers 静态资源流程，按控制台生成的配置即可）
4. 部署成功后，到项目 **Domains**，打开 **Production** 的 `workers.dev` 开关  
   （若显示 No URLs enabled，多半是开关还没开）

### 6. 修改 workers.dev 子域名（可选但推荐）

默认中间段可能带邮箱前缀数字。在 **Workers & Pages 列表页** → 右侧 **Account details → Subdomain** → 点铅笔修改。

改子域名一般**不必重新部署**；旧链接会失效，用新链接访问。

本次最终域名示例：`https://tech-notes.legp.workers.dev/`

## 以后怎么更新文章

1. 复制 `templates/写作模板.md` 到 `src/content/blog/你的文章.md`
2. 改 frontmatter 与正文
3. （可选）`npm run dev` 本地预览
4. `git add` → `git commit` → `git push`
5. 等 Cloudflare 自动构建完成，刷新线上站点

## 后期可扩展（尚未做）

- 绑定自己的域名（Domains → Add Domain）
- 站内搜索（如 Pagefind）
- 评论（如 Giscus）
- RSS

内容始终是仓库里的 Markdown，迁移成本低。
