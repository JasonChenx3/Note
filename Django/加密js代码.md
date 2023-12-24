#### Why

防止被抄。

#### How

先安装`npm`

```bash
sudo apt-get update
sudo apt-get install npm
```

再安装`terser`

```bash
mkdir npm  # 存放npm的配置文件
cd npm
npm install terser -g
```

加密：

直接输出加密后的代码，可以通过管道进行重定向输出。

```bash
terser xxx.js -c -m
```

