---
title: Charles + Mokcy 模拟后台返回数据
date: 2018.05.08 18:13
---
## 应用场景
在开发中总有一些情况，我们按照设计出的图画好了，准备调接口。然后发现后台只做了接口的字段定义，具体的数据内容并没有返回给我们。这时候又需要接口为我们提供内容做一些网络或者逻辑的调试，我们不能干等着后台给我们提供数据啊。现在我们就可以用Charles拦截请求，Mocky模拟后台数据返回给我们。
## Charles的使用
如下图勾选上 macOS Proxy，这样Charles就能获取所有通过你的电脑发出的网络请求了
![](https://upload-images.jianshu.io/upload_images/2067180-3e1f4d40edff36a6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
抓到的数据
![](https://upload-images.jianshu.io/upload_images/2067180-a0f0485191ab92fb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
### 通过Charles的的Map功能重定向到用Mokcy模拟的地址
右键你的接口选择Map Remote
![](https://upload-images.jianshu.io/upload_images/2067180-ff77429f01688a41.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
填写相关信息
![](https://upload-images.jianshu.io/upload_images/2067180-9fbff2c04b897ca7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
具体填写，请接着往下看
## Mocky模拟数据
Mocky网址https://www.mocky.io/
### Mcoky的使用
![](https://upload-images.jianshu.io/upload_images/2067180-693eacfb9e4166e4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

1. Status Code 和 Content Type如图填就行了
2. Custom headers 请求时所带的参数，多个参数就点Switch to basic mode 哪个按钮添加
3. 在Body 中构建你自己的JSON
4. 点击Generate my HTTP Response生成如下链接

![生成的链接](https://upload-images.jianshu.io/upload_images/2067180-f52c4db6ea2bb9cd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
回到Charles的重定向界面
![](https://upload-images.jianshu.io/upload_images/2067180-9fbff2c04b897ca7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

1. protocol根据具体情况填写
2. Host填写www.mokcy.io
3. path填写mokcy生成的地址
4. Query带参数请求才填，同Map From下面的Query


