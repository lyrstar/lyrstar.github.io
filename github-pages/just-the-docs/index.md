---
title: just-the-docs 主题
layout: default
parent: GitHub Pages
nav_order: 2
---

# just-the-docs 主题
{: .no_toc }

第三方文档主题，自带导航树、全文搜索与暗色模式。
{: .fs-5 .fw-300 }

<details open markdown="block">
  <summary>目录</summary>
  {: .text-delta }
- TOC
{:toc}
</details>

## 适用场景

[just-the-docs](https://github.com/just-the-docs/just-the-docs) 是为技术文档设计的主题，开箱即有：

- 左侧多级导航树，可折叠
- **纯前端全文搜索**，构建期生成索引，不依赖外部服务
- 页内目录（TOC）
- 亮色 / 暗色配色
- 代码块一键复制按钮

适合命令速查、API 文档、多分类笔记这类需要频繁翻阅的内容。若只是单页项目说明，[slate](../slate/) 这类官方主题更省事。

## 启用

just-the-docs **不在** GitHub Pages 官方支持的主题列表里，因此**不能用 `theme` 字段**，必须用 `remote_theme`：

```yaml
remote_theme: just-the-docs/just-the-docs@v0.10.1
```

`remote_theme` 由 `jekyll-remote-theme` 插件提供，该插件在 GitHub Pages 上默认启用，无需声明。

### 必需插件

主题的 layout 内部使用了 `include_cached`，**缺少 `jekyll-include-cache` 会直接构建失败**：

```yaml
plugins:
  - jekyll-include-cache
```

这是最容易漏的一步，构建日志里的报错信息也不够直观。

## 导航体系

just-the-docs 不依赖 `jekyll-readme-index`，而是**靠 front matter 显式声明导航结构**。每个页面都要写。

### 文件命名

页面文件名用 `index.md`，输出为目录形式的 URL：

```
├── index.md              → /            （首页，layout: home）
├── git/
│   ├── index.md          → /git/
│   └── ERROR.md          → /git/ERROR.html
└── linux/
    └── index.md          → /linux/
```

### 顶层页面

```markdown
---
title: linux
layout: default
nav_order: 3
---
```

### 父页面与子页面

父页声明 `has_children`：

```markdown
---
title: git
layout: default
nav_order: 2
has_children: true
---
```

子页用 `parent` 指回父页：

```markdown
---
title: 异常处理
layout: default
parent: git
nav_order: 1
---
```

### front matter 速查

| 字段 | 作用 |
| --- | --- |
| `title` | 侧边栏与浏览器标签显示的名称 |
| `layout` | 首页用 `home`，其余用 `default` |
| `nav_order` | 侧边栏排序，数字越小越靠前 |
| `parent` | 声明父页，值为父页的 `title` |
| `has_children` | 本页有子页时设为 `true` |
| `nav_exclude` | 设为 `true` 则不在侧边栏显示 |

### 页内目录

在正文开头插入，`{: .no_toc }` 让主标题本身不计入目录：

```markdown
# 页面标题
{: .no_toc }

<details open markdown="block">
  <summary>目录</summary>
  {: .text-delta }
- TOC
{:toc}
</details>
```

## 配置

```yaml
remote_theme: just-the-docs/just-the-docs@v0.10.1

plugins:
  - jekyll-include-cache

# 配色：light / dark
color_scheme: light

# 全文搜索
search_enabled: true
search:
  # 分词分隔符：保留空格和斜杠分词，
  # 使 ssh-keygen 这类带连字符的命令能整体命中
  tokenizer_separator: /[\s/]+/
  heading_level: 2
  previews: 3
  preview_words_before: 5
  preview_words_after: 10
  rel_url: true
  button: false

# 代码块右上角一键复制按钮
enable_copy_code_button: true

# 右上角外链
aux_links:
  "在 GitHub 上查看":
    - "https://github.com/<用户名>/<仓库名>"
aux_links_new_tab: true

# 每页底部「编辑此页」
gh_edit_link: true
gh_edit_link_text: "在 GitHub 上编辑此页"
gh_edit_repository: "https://github.com/<用户名>/<仓库名>"
gh_edit_branch: "master"
gh_edit_view_mode: "edit"

footer_content: "Copyright &copy; 2026 <用户名>."

back_to_top: true
back_to_top_text: "返回顶部"
```

## 首页

首页用 `layout: home`，可以使用主题提供的工具类控制字号字重，以及按钮样式：

```markdown
---
title: 首页
layout: home
nav_order: 1
---

# 站点标题
{: .fs-8 .fw-700 }

一句话描述。
{: .fs-5 .fw-300 }

[开始翻阅](git/){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }
```

常用类：`.fs-1` ~ `.fs-10` 字号、`.fw-300` / `.fw-700` 字重、`.btn` / `.btn-primary` 按钮、`.text-delta` 小标题。

## 中文适配

主题字体栈基于 `system-ui`，已含中文回退，但未显式指定中文字体，部分 Windows 浏览器会选到字形较差的宋体。在 `_includes/head-custom.html` 中前置苹方 / 微软雅黑：

```html
<style>
  /* 主题正文容器类名为 .main-content（连字符） */
  body {
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "PingFang SC",
      "Hiragino Sans GB", "Microsoft YaHei", Helvetica, Arial, sans-serif;
  }

  /* 中文段落在主题默认 line-height 下偏挤 */
  .main-content p,
  .main-content li {
    line-height: 1.8;
  }

  /* 中文标题不需要为西文设计的紧凑字距 */
  .main-content h1,
  .main-content h2,
  .main-content h3 {
    letter-spacing: normal;
  }

  /* 代码块锁回等宽字体，避免被上面的中文字体栈污染 */
  .main-content pre,
  .main-content code {
    font-family: "SFMono-Regular", Consolas, "Liberation Mono", Menlo, monospace;
  }

  /* 侧边栏导航也需要中文字体 */
  .site-nav,
  .site-title,
  .site-footer {
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "PingFang SC",
      "Hiragino Sans GB", "Microsoft YaHei", Helvetica, Arial, sans-serif;
  }
</style>
```

> 中文搜索的效果不如英文：主题的搜索基于空格分词，中文没有词边界，
> 实际表现接近「整段子串匹配」。检索命令、英文术语时很准，检索中文短语时命中率一般。

## 扩展点

主题预留了若干同名可覆盖的 include 文件。在自己仓库的 `_includes/` 下建同名文件即覆盖上游实现 —— **只能覆盖这些约定好的文件名，另起名字不会被主题包含**。

| 文件 | 注入位置 |
| --- | --- |
| `head-custom.html` | `<head>` 内，用于自定义样式、meta、统计脚本 |
| `footer_custom.html` | 正文页脚，用于版权、许可协议、备案信息 |
| `nav_footer_custom.html` | 侧边栏底部 |

### 页脚

上游的 `footer_custom.html` 仅输出 `site.footer_content`。要加许可协议之类的内容，覆盖它：

```liquid
{% raw %}{%- if site.footer_content -%}
  <p class="text-small text-grey-dk-100 mb-0">{{ site.footer_content }}</p>
{%- endif -%}

{%- if site.content_license_name and site.content_license_url -%}
  <p class="text-small text-grey-dk-100 mb-0">
    本站原创内容采用
    <a href="{{ site.content_license_url }}" rel="license noopener" target="_blank">{{ site.content_license_name }}</a>
    许可协议，转载请保留出处与原文链接。
  </p>
{%- endif -%}{% endraw %}
```

对应在 `_config.yml` 中自定义键值，想换协议只改配置不碰模板：

```yaml
footer_content: "Copyright &copy; 2026 <用户名>."
content_license_name: "CC BY-NC-SA 4.0"
content_license_url: "https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh-hans"
```

三个细节：

- **覆盖是整体替换**，上游那行 `footer_content` 输出也会消失，需要自己再写一遍
- 两个键用 `and` 同时判断，缺任一个则整条声明不输出，避免渲染出没有链接的裸协议名
- `{% raw %}{%-{% endraw %}` 的减号去除 Liquid 标签前后的空白，否则页脚会多出空行

> 正文页脚只有 `footer_custom.html` 这一个 include 点。若想改的是侧边栏底部，
> 那是另一个文件 `nav_footer_custom.html`，两者不通用。

## 注意事项

### remote_theme 必须锁版本号

```yaml
# 推荐
remote_theme: just-the-docs/just-the-docs@v0.10.1

# 不推荐：上游一旦发布不兼容更新，你的站点样式会在某次构建后突然变样
remote_theme: just-the-docs/just-the-docs
```

不写版本号时拉取的是默认分支。just-the-docs 在 v0.4 → v0.10 之间改过配置项和 CSS 类名，锁版本可以避免某天毫无改动却构建出不同结果。

### 缺 jekyll-include-cache 会构建失败

见上文[必需插件](#必需插件)。这是首次启用时最常见的失败原因。

### index.md 与 README.md 抢输出路径

GitHub Pages 默认启用 `jekyll-readme-index`，它会把 `README.md` 渲染为目录的 `index.html`。若同一目录下既有 `index.md` 又有 `README.md`，两者的输出路径冲突。

若 README 只想作为仓库说明、不参与建站，显式排除：

```yaml
exclude:
  - README.md
```

### parent 填的是父页 title，不是文件名或目录名

```markdown
---
# 父页 github-pages/index.md 的 front matter 是 title: GitHub Pages
parent: GitHub Pages     # 正确
# parent: github-pages   # 错误：这是目录名，不是 title
---
```

填错时页面不会报错，只是**静默从侧边栏消失**（既不在顶层，也不在任何父页下）—— 这个失败方式很不好排查。父页漏写 `has_children: true` 时也是同样的症状。

### 选择器是 `.main-content`，连字符

just-the-docs 的正文容器类名是 `.main-content`（**连字符的 class**）。slate 用的是 `#main_content`（**下划线的 id**），两者不通用。

从其他主题迁移过来时，原有的 `head-custom.html` 样式会整体失效且不报错 —— 页面能正常渲染，只是自定义样式一条都没生效。

### last_edit_timestamp 在 GitHub Pages 上不生效

```yaml
# 配了也没用
last_edit_timestamp: true
```

该功能依赖 `jekyll-last-modified-at`，**不在 GitHub Pages 的插件白名单内**。配置不会报错，页脚就是不显示时间。

同理，任何需要额外 gem 的主题功能（如 Mermaid 之外的自定义插件）在 GitHub Pages 的默认构建流程下都不可用。真要用，得改走 GitHub Actions 自行构建。

### 不要在 head-custom.html 里重复调用 seo 标签

主题 layout 中已调用过，重复会输出两份 title / og 标签。

且 **Liquid 在 HTML 注释里同样会被执行** —— 在 `<!-- -->` 中写出标签字面量会导致它被真正调用。确需说明时用 `raw` 包裹。

验证（结果应为 1）：

```bash
curl -sSL https://<你的域名>/ | grep -c 'Begin Jekyll SEO tag'
```

### 搜索索引是构建期生成的

搜索功能依赖构建时生成的 `search-data.json`。本地改完 md 后若搜索结果没更新，重启 `jekyll serve`；线上则需等本次构建部署完成。

验证搜索索引是否生成：

```bash
curl -sS -o /dev/null -w "%{http_code}\n" https://<你的域名>/assets/js/search-data.json
```

## 从 slate 迁移

若已有 slate 站点要换过来，需改动四处：

1. `theme: jekyll-theme-slate` → `remote_theme: just-the-docs/just-the-docs@vX.Y.Z`
2. 加 `jekyll-include-cache` 插件
3. 所有 `README.md` 改名 `index.md`，逐个补 front matter
4. `head-custom.html` 中 `#main_content` → `.main-content`

另外，slate 时期在正文顶部手写的「回到首页」类导航链接可以删掉 —— 侧边栏已经承担了这个职责，留着会和导航重复。

> 文件改名后，GitHub 仓库里浏览子目录将不再显示内容预览（那是 README 才有的行为）。
> 若在意这点，可只给子目录保留 `README.md` 而不改名，代价是要额外处理与 `index.md` 的路径冲突。
