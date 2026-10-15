- [回到首页](../README.md)
- [git异常处理](./ERROR.md)

## 官网

[打开官网](https://git-scm.com/)

### 检出仓库

```bash
git clone <repo>
```

### 拉取远程仓库

```bash
git pull [remoteName] [localBranchName]
```

### 推送远程仓库

```bash
git push [remoteName] [localBranchName]
```

### 查看本地分支

```bash
git branch
```

### 查看远程分支

```bash
git branch -r
```

### 查看工作目录和暂存区的状态

```bash
git status
```

### 显示修改的内容

```bash
git diff
```

### 将更改加入到暂存区

```bash
git add <filename>
```

### 将暂存区提交到仓库区

```bash
git commit
```

### 查看历史日志

```bash
git log
```

### 还原一个版本的修改

```bash
git revert
```

### 将暂存区的文件取消暂存

```bash
git reset
```

### 将暂存区的文件切换到指定版本

```bash
git reset --hard version
```

### 新增、更改Git设置

```bash
git config
```

- 查看用户名

    ```bash
    git config --global user.name
    ```

- 修改用户名

    ```bash
    git config --global user.name "username"
    ```

- 查看邮箱

    ```bash
    git config --global user.email
    ```

- 修改邮箱

    ```bash
    git config --global user.email example@example.com
    ```

- 查看配置列表

    ```bash
    git config --list
    ```

- 保存密码

    ```bash
    git config --global credential.helper store
    ```

### 丢弃你在本地的所有改动与提交

```bash
git fetch origin
git reset --hard origin/master
```
