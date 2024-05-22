- [回到首页](../README.md)

## ssh 免密码登录配置

- 主动访问主机生成密钥文件
    ```
    ssh-keygen -t rsa -P ''
    ```
- 将生成的 id_rsa.pub 文件内容追加到远程主机 ~/.ssh/authorized_keys 文件末尾(另起一行)
- 若远程主机没有此文件或目录,手动创建即可

***

## ubuntu配置登录问候语

```cmd
vi /etc/motd
```
