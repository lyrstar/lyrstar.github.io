---
title: slate 主题
layout: default
parent: GitHub Pages
nav_order: 1
---

# slate 主题
{: .no_toc }

GitHub 官方托管主题，一行配置即可启用。
{: .fs-5 .fw-300 }

<details open markdown="block">
  <summary>目录</summary>
  {: .text-delta }
- TOC
{:toc}
</details>

## 适用场景

slate 是 [pages-themes](https://github.com/pages-themes) 下的官方主题之一，单列布局、深色页头，适合内容不多的单页站点或项目说明页。

优点是**零成本**：不需要把主题文件放进仓库，不需要额外插件，一行配置就能用。

局限也很明确，笔记一多就会暴露：

- 没有导航栏，页头只有一个「View on GitHub」角标
- 没有搜索
- 没有页内目录（TOC）
- 没有暗色模式

内容超过十几页、需要按分类翻阅时，应考虑 [just-the-docs](../just-the-docs/)。

## 启用

官方主题由 GitHub 托管，用 `theme` 字段声明即可：

```yaml
theme: jekyll-theme-slate
```

其余官方主题（`jekyll-theme-minimal`、`jekyll-theme-cayman` 等）用法相同，可选列表见[可用主题](https://pages.github.com/themes/)。

> 官方主题仓库是公开的，可直接查看其 `_layouts` 和 `_sass`，
> 以了解有哪些可用配置项和 CSS 选择器 —— 这比猜测可靠得多。

## 配置

slate 会把 `title` 和 `description` 渲染到页头：

```yaml
theme: jekyll-theme-slate

title: 站点标题
description: "站点描述"
```

其余与主题无关的通用配置（`url`、`lang`、插件等）见[建站篇](../)。

## 组织多级笔记

依靠 GitHub Pages 默认启用的 `jekyll-readme-index` 和 `jekyll-relative-links` 两个插件，无需写任何 front matter 即可形成多级目录结构：

```
├── README.md          → 首页
├── git/
│   └── README.md      → /git/
└── linux/
    └── README.md      → /linux/
```

链接直接写相对路径指向 `.md` 文件，`jekyll-relative-links` 会改写为站内链接：

```markdown
- [git](git/README.md)
```

渲染结果为 `<a href="/git/">git</a>`。

这套做法的好处是在 GitHub 仓库里浏览子目录时会自动显示 README 预览，网站和仓库两边都能读。

> 注意：直接访问 `/git/README.md` 虽然返回 200，但 `Content-Type` 是 `text/markdown`，
> 浏览器拿到的是**未渲染的源文件**。应当让链接指向目录形式的 `/git/`。

由于 slate 没有导航栏，各页之间的跳转只能在正文顶部手写链接：

```markdown
- [回到首页](../README.md)
- [git异常处理](./ERROR.md)
```

## 中文适配

slate 的 `body` 字体栈为 `'Myriad Pro', Calibri, Helvetica, Arial`，**不含任何中文字体**，中文会退化为系统默认字体（Windows 上常落到字形较差的宋体）。

在 `_includes/head-custom.html` 中补中文字体栈：

```html
<style>
  /* 内容区容器 id 为 main_content（下划线，见主题 default.html） */
  body,
  #main_content p,
  #main_content li,
  #main_content td {
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "PingFang SC",
      "Hiragino Sans GB", "Microsoft YaHei", "Myriad Pro", Calibri, Helvetica,
      Arial, sans-serif;
  }

  /* 主题设了 letter-spacing: -1px，中文标题会显得拥挤 */
  #main_content h1,
  #main_content h2,
  #main_content h3 {
    letter-spacing: normal;
  }

  /* 主题正文 line-height 为 1.5，中文段落偏挤 */
  #main_content p,
  #main_content li {
    line-height: 1.8;
  }
</style>
```

## 注意事项

### 选择器是 `#main_content`，下划线

slate 的内容区容器 id 是 `main_content`（**下划线**），写成 `main-content`（连字符）不会匹配任何元素，样式静默失效。

不同主题的选择器各不相同（just-the-docs 用的就是 `.main-content`）。改样式前先查主题的 `_sass` 或 `_layouts` 确认，不要凭猜测。

### user page 上不显示 GitHub 角标

slate 把「View on GitHub」角标和页脚版权包在 `site.github.is_project_page` 条件里，因此 **user page 上这些元素不会显示**。

这不是数据缺失（`site.github.*` 元数据在 user page 上同样正常填充），只是条件判断挡住了渲染。

> 不要为了显示角标而把仓库改成 project page —— 代价是地址结构、域名和证书全部要重配。

### 覆盖 head-custom.html 会丢掉 Google Analytics 挂载点

slate 自带的 `_includes/head-custom.html` 里有一行 GA 挂载。在自己仓库建同名文件会**整体覆盖**它，需要自己再 include 一遍：

```html
{% raw %}{% include head-custom-google-analytics.html %}{% endraw %}
```

### 不要在 head-custom.html 里重复调用 seo 标签

主题的 `_layouts/default.html` 中已经调用过 seo 标签，此处再调一次会输出两份 title / description / og 标签。

更隐蔽的是：**Liquid 在渲染时不区分 HTML 注释，写在 `<!-- -->` 里的 Liquid 标签同样会被执行**。若在注释中提到某个标签名用于说明，会导致该标签被真正调用。确需写标签字面量时用 `raw` 包裹。

验证是否重复（结果应为 1）：

```bash
curl -sSL https://<你的域名>/ | grep -c 'Begin Jekyll SEO tag'
```

### 长命令会被折断

slate 的 `pre` 默认折行，长命令折断后容易被误复制。改为横向滚动：

```css
#main_content pre {
  overflow-x: auto;
}
```
