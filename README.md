# yilin-blog

一个基于 **Astro.js** 的个人博客和作品集网站。

仓库地址: <a href="https://github.com/Y1L1N10/yilin-blog" target="_blank"> https://github.com/Y1L1N10/yilin-blog </a>

---

### 技术栈

- Astro.js 5.12.8
- Sass 1.79.4
- typescript: 5.6.2

---

### 字体选择

中文标题字体： 汇文明朝体 <a href="https://tieba.baidu.com/p/7193815211" target="_blank">官方链接</a>

正文字体：思源黑体 <a href="https://fonts.google.com/noto/specimen/Noto+Sans+SC?query=Noto+sans+sc" target="_blank">Google Font</a>

英文字体：Special Elite <a href="https://fonts.google.com/specimen/Special+Elite" target="_blank">Google Font</a>

中文字题由于体量过大，所以其中的标题，我选择了转化为 SVG 的格式嵌入。可以自行下载字体，再转化格式替换。

---

### 基础用法说明

#### Data 网站主要信息

主要的网站信息，主要集中在 `src/data/` 下面，可以通过修改这几个文件中的数据来修改为自己的网站，

- `content.ts` 网站的基本信息，包括联系方式、导航、TDK 等
- `project.ts` 项目信息，主要展示在项目列表中，但是具体的项目详细页面，需要手动创建
- `home.json` 首页展示的作品列表，可选择展示的内容信息比较多
  About 页面需要手动添加自己的个人介绍，博客内容，只需要在 `content/blog/*`文件夹下面添加 markdown 文件即可, 按照固定格式，博客列表会自动添加标题和路径

#### 环境变量配置

`.env` 文件用于设置应用程序的环境变量。你可以从 `.env.example` 文件复制内容到 `.env` 文件，并填写所需的值。请注意，`.env` 文件不应该提交到版本控制系统，Git 会自动忽略它。

在 `.env` 文件中，你可以设置以下变量：

- `PUBLIC_SITE_URL`: 网站的公共 URL。
- `PUBLIC_SITE_NAME`: 网站名称。
- `PUBLIC_GA4_ID`: （非必需）Google Analytics 4 分析 ID。
- `PUBLIC_UMAMI_ID`: （非必需）Umami 分析 ID。

确保在本地开发和生产环境中正确设置这些变量，以便应用程序能够正常运行。

如果你希望在你的网站上启用 Google Analytics 或 Umami 分析，你需要在 `.env` 文件中设置相应的环境变量。

**Google Analytics 和 Umami 分析**

- https://analytics.google.com/
- https://umami.is/

网站主要信息存放在 `src/data/content.ts`，修改里面对应的数据即可。部分内容需要在页面中手动修改。

#### Index页面项目展示

项目数据存放地址在
`src/data/home.json`

```typescript
[
  {
    id: "1",
    cover: "/assets/cover/project-cover.jpg",
    title: "项目示例",
    desc: "项目描述",
    url: "https://example.com/",
    detail: "/detail/project",
    category: "web",
    tag: "Web",
    date: "2024-01-01",
    mark: true,
  },
];
```

```typescript
interface HomeProject {
  id?: string; // 项目id，可选填
  title: string; // 项目名称，必填
  cover?: string; // 项目封面图片，可选填
  desc?: string; // 项目描述，可选填
  url?: string; // 项目地址，可选填
  detail?: string; // 项目详细介绍页面路径，可选填
  category?: string; // 项目分类，可选填，为空则默认分类到 other
  tag?: string; // 项目标签，可选填，建议一定要填写
  date?: string; // 项目创建时间，可选填
  mark?: boolean; // 是否显示推荐标签，可选填，默认 false
}
```

#### 项目列表

项目 `/project`

```Typescript
export interface ProjectItem {
	id?: number; // 唯一标识符
	title: string // 项目名称
	title_en?: string //英文项目名称
	description?: string  // 项目描述
	date?: string		//发布日期
	detail?: string //详细页路径
  url?: string  //上线链接
	tags?: string[] //标签
	cover?: string[] //封面
}
```

#### 项目详细介绍

具体项目内容需要新建页面\*.astro，手动存档，url 在project.json 的 `detail` 属性对应填写, 不填写的话，首页项目没有链接
`src/pages/detail/`

---

### 🚀 项目结构

Inside of your Astro project, you'll see the following folders and files:

```
/
├── public/
│   ├── images/
│   ├── logo.svg
│   ├── favicon.ico
│   └── ...
├── src/
│   ├── assets/
│   │   └── *Layout.astro*
│   ├── components/
│   │   ├── *
│   ├── layouts/
│   │   └── Layout.astro
│   ├── content/
│   │   └── *
│   ├── data/
│   │   ├── content.ts
│   │   ├── project.ts
│   │   └── home.json
│   ├── effets/
│   │   └── *.astro
│   ├── layouts/
│   │   └── Layout.astro
│   └── pages/
│       ├── index.astro
│       ├── about.astro
│       ├── project.astro
│       ├── blog.astro
│       └── 404.astro
├── package.json
```

---

### 🧞 启动

Yarn Command:

| Command             | Action                                           |
| :------------------ | :----------------------------------------------- |
| `yarn install`      | Installs dependencies                            |
| `yarn dev`          | Starts local dev server at `localhost:4321`      |
| `yarn build`        | Build your production site to `./dist/`          |
| `yarn preview`      | Preview your build locally, before deploying     |
| `yarn astro ...`    | Run CLI commands like `astro add`, `astro check` |
| `yarn astro --help` | Get help using the Astro CLI                     |

---

## 更新日志

- **2025/11/06:** clone 项目并继承该项目， 师承Rico
- **2025/04/28:** 增加 effects 开源的 need3d Abstraction 系列图标，网站地址为：[need3d.ru](https://need3d.ru/)。作者是 [@ilyarygin](https://www.instagram.com/ilyarygin)。
- **2025/04/28:** 更新版本至 `Astro.js 5.7.5`， 增加 `.env.example` 文件，用于配置网站信息，默认可选 Google Analytics 4 和 Umami 分析 ID。
