# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 项目概述

这是一个基于 **Astro.js 5.12.8** 的个人作品集网站，专为设计师和开发者展示项目而设计。项目使用 TypeScript、Sass，支持 MDX 内容和响应式设计。

## 常用开发命令

```bash
#安装
yarn install     #安装依赖等

# 开发环境
npm run dev          # 启动开发服务器 (localhost:4321)
npm run start        # 同上

# 构建和部署
npm run build        # 类型检查 + 构建生产版本
npm run preview      # 预览构建后的本地版本

# Astro CLI
npm run astro ...    # 运行其他 Astro 命令
```

## 核心架构

### 目录结构

```
src/
├── components/      # 可复用组件
├── data/           # 配置和数据文件
├── pages/          # 页面路由 (Astro 文件结构)
├── content/        # MDX/Markdown 内容 (博客)
├── layouts/        # 页面布局模板
└── styles/         # 全局样式
```

### 数据管理模式

**配置文件优先**: 网站主要信息集中在 `src/data/` 下:

- `content.ts` - 站点配置、导航、SEO、社交链接
- `project.ts` - 项目列表数据 (ProjectItem 接口)
- `home.json` - 首页展示项目 (更详细字段)

**内容管理**:

- 博客内容: `src/content/blog/` 下添加 markdown 文件
- 项目详情页: 手动创建 `src/pages/detail/*.astro` 页面

### 关键接口定义

```typescript
// 项目数据接口
interface ProjectItem {
  id?: number;
  title: string;
  title_en?: string;
  description?: string;
  date?: string;
  detail?: string; // 详情页路径
  url?: string; // 外部链接
  tags?: string[];
  cover?: string[]; // 多图支持
}

// 首页项目接口 (home.json)
interface HomeProject {
  id?: string;
  title: string;
  desc?: string;
  cover?: string;
  url?: string;
  detail?: string;
  category?: string; // 分类筛选
  tag?: string;
  date?: string;
  mark?: boolean; // 推荐标记
  useVideo?: boolean;
  opensource?: boolean;
}
```

## 环境变量

在 `.env` 文件中配置 (前缀 `PUBLIC_`):

- `PUBLIC_SITE_URL` - 网站域名
- `PUBLIC_SITE_NAME` - 网站名称
- `PUBLIC_GA4_ID` - Google Analytics (可选)
- `PUBLIC_UMAMI_ID` - Umami Analytics (可选)

## 样式和主题

- 使用 **Sass** 预处理器 (modern API)
- 支持明暗主题切换 (`ThemeToggle` 组件)
- 响应式设计，移动端适配 (`NavMobile` 组件)

## 特色功能组件

- `Filter.astro` - 首页项目分类筛选
- `Search.astro` - 内容搜索功能
- `Analytics.astro` - 分析代码集成
- `Cards.astro` - 项目卡片展示
- `ProjectList.astro` - 项目列表页面

## 内容类型支持

- **MDX**: 支持 `.mdx` 文件 (已集成 @astrojs/mdx)
- **Markdown**: 博客文章使用标准 markdown
- **RSS**: 自动生成 RSS 订阅 (`/rss.xml`)

## 路径别名配置

```typescript
@/components/* -> src/components/*
@/layouts/* -> src/layouts/*
@/data/* -> src/data/*
@/styles/* -> src/styles/*
@/assets/* -> src/assets/*
```

## 构建和部署

项目支持静态站点生成，构建产物在 `dist/` 目录。可部署到任何静态托管服务 (Vercel、Netlify 等)。

## 特殊功能

### Markdown 代码高亮

使用 Shiki 主题 `github-dark`，配置在 `astro.config.mjs`。

### MDX 支持

已集成 `@astrojs/mdx`，可在博客中使用 React 组件。

### 效果组件

`src/effects/` 目录包含特效组件（如 3D Abstraction 系列图标）。

### 字体

- 中文标题：汇文明朝体（转为 SVG 嵌入）
- 正文：思源黑体（Google Fonts）
- 英文：Special Elite（Google Fonts）

## 部署注意事项

- 生产构建会自动运行 `astro check` 进行类型检查
- 确保 `.env` 文件已正确配置（不要提交到 Git）
- 静态资源放在 `public/` 目录
- 构建输出在 `./dist/`
