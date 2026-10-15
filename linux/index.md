---
title: linux
layout: default
nav_order: 3
---

# linux
{: .no_toc }

ssh 免密登录、登录问候语等配置。
{: .fs-5 .fw-300 }

<details open markdown="block">
  <summary>目录</summary>
  {: .text-delta }
- TOC
{:toc}
</details>

## ssh 免密码登录配置

- 主动访问主机生成密钥文件
    ```bash
    ssh-keygen -t rsa -P ''
    ```
- 将生成的 id_rsa.pub 文件内容追加到远程主机 ~/.ssh/authorized_keys 文件末尾(另起一行)
- 若远程主机没有此文件或目录,手动创建即可

***

## ubuntu配置登录问候语

```bash
vi /etc/motd
```
