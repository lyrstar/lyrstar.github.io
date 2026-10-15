# lyrstar.github.io

[![Pages](https://img.shields.io/badge/GitHub%20Pages-deployed-brightgreen)](https://note.lyrstar.com)

日常踩坑与命令速查的归档，便于随时翻阅。

**在线访问：[note.lyrstar.com](https://note.lyrstar.com)**

基于 [Jekyll](https://jekyllrb.com/) + [just-the-docs](https://github.com/just-the-docs/just-the-docs) 主题，由 GitHub Pages 自动构建部署。

---

## 笔记目录

| 目录 | 内容 |
| --- | --- |
| [git](git/index.md) | 常用命令速查，含[异常处理](git/ERROR.md) |
| [linux](linux/index.md) | ssh 免密登录、登录问候语等配置 |
| [nginx](nginx/index.md) | 编译安装、nginx-sticky 配置 |
| [ssl](ssl/index.md) | openssl 自签名证书 |
| [mongodb](mongodb/index.md) | 身份验证、Mongoose 相关 |
| [github-pages](github-pages/index.md) | 个人站点从零搭建：仓库、域名、HTTPS、SEO<br>· [slate 主题](github-pages/slate/index.md)<br>· [just-the-docs 主题](github-pages/just-the-docs/index.md) |
| [qq-bot](qq-bot/index.md) | QQ 开放平台与各语言 SDK 入口 |

## 目录结构

```
.
├── _config.yml              # Jekyll 与主题配置
├── _includes/
│   └── head-custom.html     # 中文字体等样式覆盖
├── CNAME                    # 自定义域名
├── index.md                 # 网站首页（layout: home）
├── README.md                # 仓库说明（已 exclude，不参与建站）
├── robots.txt
└── <分类>/
    ├── index.md             # 该分类笔记，输出为 /<分类>/
    └── <子分类>/
        └── index.md         # 子页，需在 front matter 声明 parent
```

## 新增一篇笔记

### 新增一个分类

建目录并放 `index.md`，补上 front matter：

```markdown
---
title: redis
layout: default
nav_order: 9
---

# redis
{: .no_toc }

一句话描述。
{: .fs-5 .fw-300 }

<details open markdown="block">
  <summary>目录</summary>
  {: .text-delta }
- TOC
{:toc}
</details>

## 第一节
```

然后在 `index.md` 的笔记目录和本文件的表格里各加一行。

### 给已有分类加子页

在该分类目录下新建 `.md`，用 `parent` 指向父页的 `title`：

```markdown
---
title: 异常处理
layout: default
parent: git
nav_order: 1
---
```

父页的 front matter 需要有 `has_children: true`（参考 `git/index.md`）。

### front matter 速查

| 字段 | 作用 |
| --- | --- |
| `title` | 侧边栏与浏览器标签显示的名称 |
| `layout` | 首页用 `home`，其余用 `default` |
| `nav_order` | 侧边栏排序，数字越小越靠前 |
| `parent` | 声明父页，值为父页的 `title` |
| `has_children` | 本页有子页时设为 `true` |
| `nav_exclude` | 设为 `true` 则不在侧边栏显示 |

### 注意事项

- **站内链接指向目录形式**（`../ssl/`），不要写 `../ssl/README.md`。后者虽返回 200，但 `Content-Type` 为 `text/markdown`，浏览器拿到的是未渲染的源文件。
- `_includes/head-custom.html` 中**不要调用 `seo` 标签**，主题 layout 已调用过，重复会输出两份 meta。Liquid 在 HTML 注释里同样会被执行，注释中也不要出现 Liquid 标签字面量。

## 本地预览

需要 Ruby 环境。首次运行先建 `Gemfile`：

```ruby
source "https://rubygems.org"
gem "github-pages", group: :jekyll_plugins
```

```bash
bundle install
bundle exec jekyll serve
```

访问 <http://127.0.0.1:4000>。

> `Gemfile` 与 `Gemfile.lock` 已在 `_config.yml` 的 `exclude` 中，不会被部署。
> 直接 push 由 GitHub Pages 构建也可以，本地预览只是为了少等一轮 CI。

## License

笔记内容仅供个人参考。
