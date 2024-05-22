- [回到首页](/README.md)

## openssl 简单的https验证 自签名证书

```cmd
openssl genrsa -out cert.key 2048

openssl req -new -key cert.key -out cert.csr
openssl x509 -req -days 1000 -in cert.csr -signkey cert.key -out cert.pem
```
