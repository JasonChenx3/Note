# JavaScript笔记

## Linux加密js代码

```bash
# 安装npm
sudo apt-get update
sudo apt-get install npm

# 安装tensor
mkdir npm  # 存放npm的配置文件
cd npm
npm install terser -g

# 加密
# 直接输出加密后的代码，可以通过管道进行重定向输出。
terser xxx.js -c -m
```
