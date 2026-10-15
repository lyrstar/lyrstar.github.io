---
title: GitHub Pages
layout: default
nav_order: 7
---

# GitHub Pages
{: .no_toc }

个人站点从零搭建：域名、HTTPS、SEO。
{: .fs-5 .fw-300 }

<details open markdown="block">
  <summary>目录</summary>
  {: .text-delta }
- TOC
{:toc}
</details>

## 官网

[打开官网](https://pages.github.com/)

[官方文档](https://docs.github.com/pages)

[可用主题](https://pages.github.com/themes/)

***

## 站点类型：user page 与 project page

动手前必须先确定建哪一种，**类型由仓库名决定，事后更改会导致地址变更**。

| | user page | project page |
|---|---|---|
| 仓库名 | 必须是 `<用户名>.github.io` | 任意名称 |
| 数量 | 每个账号只能有一个 | 不限 |
| 访问地址 | `https://<用户名>.github.io/` | `https://<用户名>.github.io/<仓库名>/` |
| `baseurl` | 留空 | 必须设为 `/<仓库名>` |
| `site.github.is_project_page` | `false` | `true` |

个人博客/主页选 user page，软件项目的文档站选 project page。

> 踩坑：部分主题（如 slate）把「View on GitHub」角标和页脚版权包在 `is_project_page` 条件里，
> 因此 user page 上这些元素**不会显示**。这不是数据缺失（`site.github.*` 元数据在
> user page 上同样正常填充），只是条件判断挡住了渲染。不要为了显示角标而把仓库
> 改成 project page，代价是地址结构、域名和证书全部要重配。

***

## 创建仓库并启用

1. 新建仓库，user page 的仓库名必须为 `<用户名>.github.io`

2. 仓库 **Settings → Pages → Build and deployment**

    - Source 选 `Deploy from a branch`
    - Branch 选 `master`（或 `main`），目录选 `/ (root)`

3. 推送任意 `index.html` 或 `README.md` 即可生成首页

***

## 选择主题

最省事的方式是用 GitHub 官方托管的主题，只需在 `_config.yml` 中声明一行，无需把主题文件放进仓库：

```yaml
theme: jekyll-theme-slate
```

官方主题仓库在 [pages-themes](https://github.com/pages-themes)，可直接查看其 `_layouts` 和 `_sass`
以了解可用的配置项和 CSS 选择器。

***

## 配置 _config.yml

```yaml
theme: jekyll-theme-slate

# 多数主题会把 title 和 description 渲染到页头
title: 站点标题
description: "站点描述"

# 必须与自定义域名一致，否则 canonical 链接和 sitemap 会指向错误域名
# 结尾不要加斜杠，否则生成的链接会出现双斜杠
url: "https://<你的域名>"
baseurl: ""

# 中文站点需显式声明，影响 <html lang> 与 og:locale
lang: zh-CN

author: <用户名>

plugins:
  - jekyll-seo-tag
  - jekyll-sitemap

markdown: kramdown
kramdown:
  input: GFM
  syntax_highlighter: rouge
```

> 踩坑：`url` 写成 `https://example.com/`（带结尾斜杠）且 `baseurl` 为空时，
> 生成的 canonical 会是 `https://example.com//`，多一个斜杠。

### 插件说明

GitHub Pages 只允许使用[白名单插件](https://pages.github.com/versions/)，其中一部分**默认就已启用**，无需声明：

| 插件 | 作用 |
|---|---|
| `jekyll-readme-index` | 把目录下的 `README.md` 渲染为该目录的 `index.html` |
| `jekyll-relative-links` | 把指向 `.md` 文件的链接自动改写为站内页面链接 |
| `jekyll-optional-front-matter` | 没有 front matter 的 `.md` 也会被渲染 |

另一些虽在白名单内，但**必须显式声明才会生效**：

| 插件 | 作用 |
|---|---|
| `jekyll-seo-tag` | 生成 title、meta description、Open Graph 等标签 |
| `jekyll-sitemap` | 生成 `/sitemap.xml` |

***

## 用 README.md 组织多级笔记

依靠上面两个默认插件，无需写任何 front matter 即可形成多级目录结构：

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

> 注意：直接访问 `/git/README.md` 虽然返回 200，但 `Content-Type` 是 `text/markdown`，
> 浏览器拿到的是**未渲染的源文件**。应当让链接指向目录形式的 `/git/`。

***

## 绑定自定义域名

### 1. 仓库侧

在仓库根目录创建 `CNAME` 文件，内容为一行域名（不含协议和斜杠）：

```
www.example.com
```

也可在 **Settings → Pages → Custom domain** 填写，GitHub 会自动提交该文件。

> 踩坑：**不要删除或清空 `CNAME` 文件**，它是 GitHub 识别自定义域名并维持证书的依据，
> 误删会导致域名配置丢失、证书失效，需要重新签发。

### 2. DNS 侧

**使用 www 等子域名**（推荐，配置最简单）——添加一条 CNAME 记录：

```
www    CNAME    <用户名>.github.io
```

**使用裸域名**（如 `example.com`）——需添加 4 条 A 记录：

```
@    A    185.199.108.153
@    A    185.199.109.153
@    A    185.199.110.153
@    A    185.199.111.153
```

如需 IPv6，再加 4 条 AAAA 记录：

```
@    AAAA    2606:50c0:8000::153
@    AAAA    2606:50c0:8001::153
@    AAAA    2606:50c0:8002::153
@    AAAA    2606:50c0:8003::153
```

配置完成后，原 `<用户名>.github.io` 会自动 301 跳转到自定义域名。

***

## 配置 HTTPS

**不需要自己申请或上传任何证书。** GitHub Pages 在检测到 `CNAME` 且 DNS 解析正确后，
会自动通过 Let's Encrypt 签发证书并在到期前自动续期。

签发完成后，到 **Settings → Pages** 勾选 **Enforce HTTPS**，HTTP 访问才会 301 跳转到 HTTPS。

> 该选项在证书签发完成前是灰色不可点的，等几分钟刷新即可。
> 勾选后生效也有延迟，实测约 2 分钟。

查看当前证书信息：

```bash
echo | openssl s_client -servername <你的域名> -connect <你的域名>:443 2>/dev/null \
  | openssl x509 -noout -subject -issuer -dates
```

> 区别于自签名证书：[openssl 自签名](../ssl/)适用于本地或内网测试，浏览器会提示不受信任；
> GitHub Pages 这种是 CA 签发的受信任证书，全自动，两者不要混用。

***

## 自定义主题样式

不建议直接复制整个主题。官方主题都提供了 `_includes/head-custom.html` 作为扩展点，
在自己仓库中建同名文件即可覆盖，内容会被注入到 `<head>`：

```html
{% raw %}{% include head-custom-google-analytics.html %}{% endraw %}

<style>
  /* 自定义样式，选择器需与主题实际结构一致 */
</style>
```

两个关键细节：

- 覆盖后主题原有的内容会整体失效，如需保留其中的挂载点要自己再 include 一遍
- 修改样式前先查看主题的 `_sass` 文件确认选择器，不要凭猜测。
  例如 slate 的内容区容器 id 是 `main_content`（下划线），写成 `main-content` 不会匹配

> 踩坑：**Liquid 在渲染时不区分 HTML 注释，写在 `<!-- -->` 里的 Liquid 标签同样会被执行。**
> 若在注释中提到某个标签名用于说明，会导致该标签被真正调用。
> 典型后果是主题 layout 已调用过的 seo 标签被重复执行，页面输出两份 title 和 og 标签。
> 确需在注释里写标签字面量时，用 `raw` 标签包裹。

***

## 验证命令

```bash
# 跳转链路与最终地址
curl -sSL -o /dev/null -w "final=%{url_effective} code=%{http_code} redirects=%{num_redirects}\n" http://<你的域名>

# HTTP 是否已强制跳转到 HTTPS
curl -sS -o /dev/null -w "%{http_code} -> %{redirect_url}\n" http://<你的域名>

# sitemap 是否生成
curl -sS -o /dev/null -w "%{http_code}\n" https://<你的域名>/sitemap.xml

# 检查 lang、canonical 等 head 输出
curl -sSL https://<你的域名>/ | grep -iE '<html lang|canonical|og:locale'

# SEO 标签是否重复（结果应为 1）
curl -sSL https://<你的域名>/ | grep -c 'Begin Jekyll SEO tag'

# DNS 解析是否指向 GitHub Pages
nslookup <你的域名>
```

构建失败或未更新时，到仓库 **Actions** 页查看 `pages build and deployment` 的日志。
