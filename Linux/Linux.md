# Linux

## 免密登录

Server端使用下面命令生成`.ssh`文件夹，里面包含公钥和私钥。

```bash
ssh-keygen
```

一直回车就行。

进入`.ssh`文件夹。

创建`authorized_keys`。

```bash
vim authorized_keys
```

再将本地的公钥复制进去。

## 创建用户

```bash
# 创建用户
adduser [username]

# 分配sudo权限
usermod -aG sudo [useranem]

# 上传配置文件
# server_name需要换成自己配置的别名
scp .bashrc .vimrc .tmux.conf user@host:

# 安装tmux
sudo apt-get update
sudo apt-get install tmux
```
