#### 安装 `PicGo-Core(command line)`

1. 打开 `Typora` ，点击左上角文件，再点击偏好设置，（或者使用快捷键`Crtl + ,`），再点击左侧图像，上传服务选择 `PicGo-Core(command line)` 。

![image-20231018153332174](https://s2.loli.net/2023/10/18/FdvnyzWlXpV32OH.png)

然后点击 `打开配置文件` 。

我的电脑没有反应。

但配置文件是在 `.picgo` 的 `config.json` 文件中，用 Everything 搜索一下就行。

![image-20231018153553129](https://s2.loli.net/2023/10/18/LJSrukVCZcBYqb7.png)

打开 `config.json`。

![image-20231018153713352](https://s2.loli.net/2023/10/18/Lf2K7CwNgumU1bY.png)

配置文件内容如下：

```json
{
  "picBed": {
    "uploder": "smms",
  "smms":{
   "token":"自己的token"
 }
  },
  "picgoPlugins": {}
}
```

#### 注册图床网站

这个[网站](https://sm.ms/)。

![image-20231018154045196](https://s2.loli.net/2023/10/18/JXhumecnV182vt4.png)

免费容量是5GB，对于我来说完全够用了。

注册完之后选择 `API Token` ，如下图。

![image-20231018154240041](https://s2.loli.net/2023/10/18/kE6LthMygaz7GKF.png)

再点击 `Generate Secret Token` ，如下图。

![image-20231018154715422](https://s2.loli.net/2023/10/18/cB4xI5wlyekVFD8.png)

然后复制 `Secret Token` ，粘贴到配置文件的 `token` 处。

#### 异常情况

1. 验证图片上传选项提示Failed。

![image-20231018160500931](https://s2.loli.net/2023/10/18/9fIHUlAPCK2hr8i.png)

这个不影响使用。
