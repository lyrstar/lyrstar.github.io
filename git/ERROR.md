- [回到首页](../README.md)
- [git学习笔记](./README.md)


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
