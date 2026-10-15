---
title: 异常处理
layout: default
parent: git
nav_order: 1
---

# git 异常处理
{: .no_toc }

<details open markdown="block">
  <summary>目录</summary>
  {: .text-delta }
- TOC
{:toc}
</details>

## 错误
- 指令

```bash
git pull
```

- 报错

```
remote: HTTP Basic: Access denied
fatal: Authentication failed for 'https://gitlab.xxx.com/xxx/xxx.git/'
```

- 解决

```bash
git config --system --unset credential.helper
```
