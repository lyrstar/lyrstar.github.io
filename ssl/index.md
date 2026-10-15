---
title: ssl
layout: default
nav_order: 5
---

# ssl
{: .no_toc }

openssl 自签名证书。
{: .fs-5 .fw-300 }

<details open markdown="block">
  <summary>目录</summary>
  {: .text-delta }
- TOC
{:toc}
</details>

## openssl 简单的https验证 自签名证书

```bash
openssl genrsa -out cert.key 2048

openssl req -new -key cert.key -out cert.csr
openssl x509 -req -days 1000 -in cert.csr -signkey cert.key -out cert.pem
```
