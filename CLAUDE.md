# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 项目概述

这是一个基于 Astro.js 5.12.8 的个人博客和作品集网站，使用 TypeScript 和 Sass 构建。

### 技术栈

- **Astro.js**: 5.12.8（静态站点生成器）
- **TypeScript**: 5.6.2（严格模式）
- **Sass**: 1.79.4（CSS 预处理器，使用 modern API）
- **集成插件**:
  - `@astrojs/sitemap` - 自动生成站点地图
  - `@astrojs/rss` - RSS 订阅功能
  - `@astrojs/mdx` - MDX 支持（Markdown + JSX）

### 字体方案

- **中文标题**: 汇文明朝体（转为 SVG 格式嵌入，避免字体文件过大）
- **正文**: 思源黑体 (Noto Sans SC)
- **英文**: Special Elite

## 常用命令

### 使用 Yarn

```bash
# 安装依赖
yarn install

# 启动开发服务器 (localhost:4321)
yarn dev

# 构建生产版本 (包含类型检查)
yarn build

# 预览构建结果
yarn preview

# Astro CLI 命令
yarn astro [command]
```

### 使用 npm

```bash
# 安装依赖
npm install

# 启动开发服务器 (localhost:4321)
npm run dev

# 构建生产版本 (包含类型检查)
npm run build

# 预览构建结果
npm run preview

# Astro CLI 命令
npm run astro [command]
# 或者使用 npx
npx astro [command]
```

**常用 Astro CLI 命令示例**:
```bash
# 添加集成（如 React、Vue 等）
npm run astro add [integration]

# 检查项目（类型检查）
npm run astro check

# 查看帮助
npm run astro --help
```

## Astro 配置

### 核心配置 (astro.config.mjs)

```javascript
{
  markdown: {
    shikiConfig: {
      theme: "github-dark",  // 代码高亮主题
      wrap: true             // 代码自动换行
    }
  },
  envPrefix: 'PUBLIC_',      // 环境变量前缀
  site: SITE_URL,            // 站点 URL（用于 sitemap 和 RSS）
  base: '/',                 // 基础路径
  integrations: [
    sitemap(),               // 自动生成 sitemap.xml
    mdx()                    // 支持 MDX 文件
  ]
}
```

### RSS 订阅

项目支持 RSS 订阅，订阅地址：`/rss.xml`

## 核心架构

### 内容数据管理

项目采用双数据源架构：

1. **静态配置数据** (`src/data/`)
   - `content.ts` - 网站全局配置（导航、SEO、社交链接、页面标签、筛选项等）
   - `project.ts` - 项目列表数据，定义 `ProjectItem` 接口
   - `home.json` - 首页作品展示数据，定义 `HomeProject` 接口

2. **Astro Content Collections** (`src/content/`)
   - `blog/` - Markdown/MDX 博客文章（支持 .md 和 .mdx）
   - `config.ts` - 定义内容集合 schema（使用 Zod 验证）

### TypeScript 路径别名

```typescript
@/components/* → src/components/*.astro
@/layouts/*    → src/layouts/*.astro
@/data/*       → src/data/*
@/styles       → src/styles/
@/assets/*     → src/assets/*
```

### 布局系统

- `BaseLayout.astro` - 基础布局模板（包含头部、导航、页脚）
- `BlogPostLayout.astro` - 博客文章布局（继承 BaseLayout）
- `DetailPostLayout.astro` - 项目详情页布局（继承 BaseLayout）

### 主要组件

**导航相关**:
- `Nav.astro` - 桌面端导航栏
- `NavMobile.astro` - 移动端导航栏
- `ThemeToggle.astro` - 明暗主题切换

**内容展示**:
- `Cards.astro` - 项目卡片组件
- `ProjectList.astro` - 项目列表
- `PostPreview.astro` - 博客文章预览
- `Filter.astro` - 首页作品筛选器
- `Search.astro` - 搜索组件

**功能组件**:
- `Analytics.astro` - 统计分析（支持 GA4 和 Umami）
- `Social.astro` - 社交链接
- `Footer.astro` - 页脚
- `Back.astro` - 返回按钮
- `Icon.astro` - 图标组件

### 页面路由

- `/` - 首页（作品展示 + 筛选功能）
- `/project` - 项目列表页
- `/blog` - 博客列表页
- `/blog/[...slug]` - 动态博客文章页
- `/about` - 关于页面
- `/detail/*` - 手动创建的项目详情页（需要与 home.json 或 project.ts 中的 detail 字段对应）
- `/404` - 404 错误页面
- `/rss.xml` - RSS 订阅源（自动生成）
- `/sitemap-index.xml` - 站点地图（自动生成）

### 环境变量配置

必须从 `.env.example` 复制到 `.env` 并配置：

```env
PUBLIC_SITE_URL=    # 网站 URL（必填，用于 sitemap 和 RSS）
PUBLIC_SITE_NAME=   # 网站名称（必填）
PUBLIC_GA4_ID=      # Google Analytics 4 ID（可选）
PUBLIC_UMAMI_ID=    # Umami 分析 ID（可选）
```

**注意**:
- `.env` 文件已被 Git 忽略，不会提交到版本控制
- 环境变量必须以 `PUBLIC_` 开头才能在客户端访问
- 分析功能是可选的，不配置不影响网站运行

## 数据结构定义

### HomeProject 接口 (首页作品)

```typescript
interface HomeProject {
  id?: string;           // 项目 ID（可选，但建议填写唯一值）
  title: string;         // 项目名称（必填）
  cover?: string;        // 封面图路径，如 "/assets/cover/xxx.jpg"
  desc?: string;         // 项目描述
  url?: string;          // 外部链接（项目在线地址）
  detail?: string;       // 详情页路径，如 "/detail/project-name"
  category?: string;     // 分类，支持多分类用逗号分隔，如 "web,ui,recommend"
  tag?: string;          // 显示标签（单个）
  date?: string;         // 日期，格式 "YYYY-MM-DD"
  mark?: boolean;        // 是否显示推荐标签（默认 false）
  useVideo?: boolean;    // 是否使用视频封面（可选）
  opensource?: boolean;  // 是否开源项目（可选）
}
```

### ProjectItem 接口 (项目列表)

```typescript
interface ProjectItem {
  id?: number;           // 唯一标识符
  title: string;         // 项目名称（必填）
  title_en?: string;     // 英文项目名称
  description?: string;  // 项目描述
  date?: string;         // 发布日期 "YYYY-MM-DD"
  detail?: string;       // 详情页路径，如 "/detail/project-name"
  url?: string;          // 上线链接
  tags?: string[];       // 标签数组，如 ["WEB", "UI", "3D"]
  cover?: string[];      // 封面图数组，多图展示
}
```

### BlogPost Schema (博客文章)

```typescript
{
  title: string;          // 文章标题（必填）
  description?: string;   // 文章描述（可选）
  publishDate: Date;      // 发布日期（必填，自动转换为日期对象）
  read?: number;          // 阅读时间（分钟）
  tags?: string[];        // 标签数组
  img?: string;           // 文章封面图
  img_alt?: string;       // 封面图 alt 文本
}
```

## 内容添加指南

### 添加博客文章

在 `src/content/blog/` 创建 `.md` 或 `.mdx` 文件：

**Markdown 示例** (.md):
```yaml
---
title: 文章标题
description: 文章描述（可选）
publishDate: 2024-01-01
tags: [技术, 前端, Astro]
img: /assets/blog/cover.jpg
img_alt: 封面图描述
read: 5
---

文章内容使用 Markdown 语法...
```

**MDX 示例** (.mdx):
```yaml
---
title: 使用 React 组件的文章
publishDate: 2024-01-01
---

import CustomComponent from '@/components/CustomComponent.astro'

文章内容可以混合使用 Markdown 和 JSX 组件...

<CustomComponent />
```

**注意事项**:
- `publishDate` 是必填字段
- 图片路径使用 `/assets/` 开头的绝对路径
- 代码块会使用 `github-dark` 主题高亮
- 文章会自动出现在博客列表页和 RSS 订阅中

### 添加首页作品

编辑 `src/data/home.json`，添加项目数据：

```json
{
  "id": "10",
  "cover": "/assets/cover/project.jpg",
  "title": "项目名称",
  "desc": "项目简短描述",
  "url": "https://example.com",
  "detail": "/detail/project-name",
  "category": "web,recommend",
  "tag": "Web",
  "date": "2024-01-01",
  "mark": true
}
```

**字段说明**:
- **id**: 建议使用唯一值，避免重复
- **category**: 支持多分类，用逗号分隔。常用分类：
  - `recommend` - 推荐作品（对应筛选器"✨推荐"）
  - `web` - 网页项目
  - `ui` - UI 设计
  - `3d` - 3D 作品
  - `photography` - 摄影
  - `brand` - 品牌设计
  - `ai` - AI 相关
- **detail**: 如填写外部链接，点击会跳转到外部；如填写 `/detail/xxx`，需手动创建详情页
- **mark**: 设为 `true` 会在卡片上显示推荐角标

**筛选器配置**:
编辑 `src/data/content.ts` 中的 `filterItems` 数组，根据需要启用/禁用分类筛选项。

### 添加项目详情页

**步骤**:

1. 在 `src/pages/detail/` 创建 `project-name.astro` 文件
2. 使用 `DetailPostLayout` 布局模板
3. 在 `home.json` 或 `project.ts` 的 `detail` 字段填写路径 `/detail/project-name`

**示例代码**:
```astro
---
import DetailPostLayout from '@/layouts/DetailPostLayout.astro';
---

<DetailPostLayout title="项目名称" description="项目描述">
  <!-- 项目详细内容 -->
  <section>
    <h2>项目背景</h2>
    <p>项目介绍...</p>
  </section>

  <section>
    <h2>技术栈</h2>
    <ul>
      <li>Astro.js</li>
      <li>TypeScript</li>
    </ul>
  </section>
</DetailPostLayout>
```

### 添加项目列表项

编辑 `src/data/project.ts`，在 `projectItems` 数组中添加：

```typescript
{
  id: 1,
  title: "项目标题",
  title_en: "Project Title",
  description: "项目描述",
  date: "2024-01-01",
  detail: "/detail/project-name",
  url: "https://example.com",
  tags: ["WEB", "UI", "3D"],
  cover: ["project/01.jpg", "project/02.jpg"]
}
```

**注意**:
- `cover` 图片路径相对于 `public/assets/` 目录
- 可以添加多张封面图，会以轮播或网格形式展示

### 修改网站信息

编辑 `src/data/content.ts`，所有网站配置集中在此文件：

**导航栏** (`nav` 对象):
```typescript
{
  avatar: "/assets/author.jpg",  // 头像路径
  items: [
    { label: "首页", href: "/", target: "_self" },
    { label: "项目", href: "/project" },
    // target: "_blank" 可在新标签页打开
  ]
}
```

**SEO TDK** (各页面元信息):
- `homeTdk` - 首页 SEO
- `blogTdk` - 博客列表页 SEO
- `aboutTdk` - 关于页面 SEO
- `projectTdk` - 项目页面 SEO
- `notFoundTdk` - 404 页面 SEO

**社交链接** (`socialLinks` 数组):
```typescript
{
  name: "Github",
  url: "https://github.com/username",
  icon: `<svg>...</svg>`  // SVG 图标代码
}
```

**页面描述** (`pageDescription` 对象):
设置各页面大标题下的说明文字

**筛选项** (`filterItems` 数组):
配置首页作品筛选器，对应 `HomeProject` 的 `category` 字段

## 开发注意事项

### 内容管理

1. **数据文件修改**: 大部分内容修改通过更新 `src/data/` 下的配置文件即可，无需修改代码
2. **新增页面**: Astro 使用文件系统路由，在 `src/pages/` 创建 `.astro` 文件即自动生成对应路由
3. **博客自动化**: Markdown 文件会自动被 Content Collections 处理，生成列表和 RSS

### 类型安全

4. **TypeScript 严格模式**: 项目使用严格模式，注意类型提示
5. **接口定义**: 修改数据结构时，同步更新对应的 TypeScript 接口
6. **Zod Schema**: 博客文章的 frontmatter 会被 `src/content/config.ts` 中的 schema 验证

### 资源管理

7. **图片资源**:
   - 封面图、头像等静态资源放在 `public/assets/` 目录
   - 路径使用 `/assets/` 开头的绝对路径
   - 支持 JPG、PNG、WebP 等常见格式

8. **字体处理**: 中文标题字体已转为 SVG，避免大文件加载

### SEO 和分析

9. **SEO 配置**: 每个页面的 TDK 在 `src/data/content.ts` 中对应配置
10. **Sitemap**: 构建时自动生成 `sitemap-index.xml`
11. **RSS 订阅**: 自动生成 `rss.xml`，包含所有博客文章
12. **统计分析**: 支持 Google Analytics 4 和 Umami，通过环境变量配置

### 常见问题

**Q: home.json 中的 ID 重复会有什么问题？**
A: 虽然不会报错，但可能导致筛选和排序异常，建议保持唯一性。

**Q: 如何添加新的分类筛选项？**
A: 编辑 `src/data/content.ts` 的 `filterItems` 数组，添加新的筛选项，然后在 `home.json` 的项目 `category` 字段中使用对应的 `dataGroup` 值。

**Q: 博客文章不显示？**
A: 检查 frontmatter 是否包含必填字段 `title` 和 `publishDate`，且 `publishDate` 格式是否正确。

**Q: 如何修改代码高亮主题？**
A: 编辑 `astro.config.mjs`，修改 `markdown.shikiConfig.theme` 为其他 Shiki 主题名称。

**Q: 本地开发时环境变量不生效？**
A: 确保 `.env` 文件存在且环境变量以 `PUBLIC_` 开头，修改后需重启开发服务器。

## 项目结构概览

```
/
├── public/
│   ├── assets/          # 静态资源（图片、字体等）
│   │   ├── cover/       # 项目封面图
│   │   └── author.jpg   # 头像
│   ├── favicon.ico
│   └── robots.txt
├── src/
│   ├── assets/          # 需要处理的资源
│   ├── components/      # Astro 组件
│   │   ├── functions/   # 功能性组件（Analytics 等）
│   │   └── *.astro
│   ├── content/         # Content Collections
│   │   ├── blog/        # 博客文章（.md / .mdx）
│   │   └── config.ts    # Collections Schema
│   ├── data/            # 静态数据配置
│   │   ├── content.ts   # 网站全局配置
│   │   ├── project.ts   # 项目列表
│   │   └── home.json    # 首页作品
│   ├── effects/         # 特效组件（可选）
│   ├── layouts/         # 布局模板
│   │   ├── BaseLayout.astro
│   │   ├── BlogPostLayout.astro
│   │   └── DetailPostLayout.astro
│   ├── pages/           # 页面路由
│   │   ├── detail/      # 项目详情页
│   │   ├── blog/
│   │   │   └── [...slug].astro  # 动态博客路由
│   │   ├── index.astro  # 首页
│   │   ├── project.astro
│   │   ├── blog.astro
│   │   ├── about.astro
│   │   └── 404.astro
│   └── styles/          # 全局样式（Sass）
├── .env.example         # 环境变量模板
├── astro.config.mjs     # Astro 配置
├── tsconfig.json        # TypeScript 配置
├── package.json
├── README.md
└── CLAUDE.md            # 本文档
```

## 快速上手流程

1. **环境准备**:
   ```bash
   # 复制环境变量
   cp .env.example .env

   # 编辑 .env 填写必填项
   # PUBLIC_SITE_URL 和 PUBLIC_SITE_NAME
   ```

2. **安装依赖**:
   ```bash
   yarn install
   ```

3. **启动开发**:
   ```bash
   yarn dev
   # 访问 http://localhost:4321
   ```

4. **个性化配置**:
   - 修改 `src/data/content.ts` 更新网站信息
   - 替换 `public/assets/author.jpg` 为自己的头像
   - 编辑 `src/pages/about.astro` 添加个人介绍
   - 删除 `src/data/home.json` 和 `src/data/project.ts` 中的示例项目

5. **添加内容**:
   - 在 `src/content/blog/` 添加博客文章
   - 在 `src/data/home.json` 添加首页作品
   - 在 `src/pages/detail/` 创建项目详情页

6. **构建部署**:
   ```bash
   yarn build    # 构建生产版本
   yarn preview  # 本地预览构建结果
   ```

## 最佳实践

### 性能优化

- **图片优化**: 使用 WebP 格式，压缩图片大小
- **懒加载**: 大量图片的页面建议使用懒加载
- **代码分割**: Astro 自动进行代码分割，无需手动配置

### 内容管理

- **ID 唯一性**: `home.json` 中的 `id` 建议使用递增数字或时间戳
- **分类规范**: 统一使用小写英文作为 `category` 值，多分类用逗号分隔
- **日期格式**: 统一使用 `YYYY-MM-DD` 格式

### 代码规范

- **组件命名**: 使用 PascalCase（如 `PostPreview.astro`）
- **路径别名**: 优先使用 `@/` 别名而非相对路径
- **类型定义**: 导出接口供其他模块使用

## 更新日志参考

当前项目基于以下版本和更新：

- **2025/11/06**: Fork 自 Rico 的项目模板
- **2025/04/28**: 升级到 Astro.js 5.7.5
- **2025/04/28**: 增加环境变量配置支持
