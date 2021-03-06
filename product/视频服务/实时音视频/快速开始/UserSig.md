为了避免有恶意攻击者盗用您的通话流量，腾讯云实时音视频（TRTC）服务需要您正确的生成 UserSig 才能顺利使用，本文主要介绍 UserSig 的作用和生成方法。

<h2 id="UserSig"> UserSig 介绍 </h2>

- UserSig 由客户根据 sdkAppId，UserID，客户指定的签名过期时间，用非对称加密运算生成。

- 生成的 UserSig 会通过接口参数传递给 TRTC SDK，TRTC SDK 再将其同 sdkAppId 和 UserID 一并提交给腾讯云的音视频服务器。

- 腾讯云会验证 UserSig 的合法性，验证如果不通过会拒绝 SDK 的连接请求。

- 由于 UserSig 是使用非对称加密计算生成的，只要您不泄漏您的私钥，您的流量就很难被盗刷。所以我们强烈建议您将计算 UserSig 的模块放在您的服务器上。

![](https://main.qcloudimg.com/raw/1efb31287f84a68980f5fcc7df289cec.png)


<h2 id="Generate"> 生成 UserSig </h2>

<h3 id="PrivateKey"> 下载签名用的私钥 </h3>

进入腾讯云实时音视频[控制台](https://console.cloud.tencent.com/rav)，如果还没有应用就创建一个，之后进入**快速上手**页面，既可以获得签名用的私钥下载链接：

![](https://main.qcloudimg.com/raw/8d4b35085f3e774d70f403f92d273d4b.png)

点击下载，会得到一个叫做 **keys.zip** 的压缩文件，解压后会获得一个叫做 **private_key** 和一个叫做 **public_key** 的文件，其中 **private_key** 就是我们所需要的私钥文件。

![](https://main.qcloudimg.com/raw/95875a7baca63c21103bc6cd6dac0279.png)

<h3 id="GetForDebug"> 控制台手动生成 </h3>

在 **快速上手** 页面，将私钥文件拷贝到第三步的文本框中，再点击**生成Demo配置文件内容**的按钮，即可获得一个json文件，文件中有一组 userid 和 usersig。

![](https://main.qcloudimg.com/raw/5de8161bb72b2e19ebdb24ef6056751c.png)

这一批 userid 和 usersig 可以直接用在我们提供的 Demo 中，便于您快速开始测试和调试。但真正要发布上线，并不适合采用这种做法，推荐采用服务端计算的方案。

<h3 id="GetFromServer"> 在服务器上计算</h3>

#### 计算原理

UserSig 是使用上面提到的 **private_key** 对 sdkappid、userid 和 expire（签名过期时间）进行非对称加密。

```
  UserSig = 非对称加密（private_key, sdkappid, userid, expire）
```

#### 源码下载

我们提供了 java、php 和 nodejs 三个版本的 UserSig 计算代码，您可以直接下载并集成到您的服务端。

| 语言版本 | 关键函数 | 下载链接 |
|:---------:|:---------:|:---------:|
| java | `genUserSig` | [Github](https://github.com/TencentVideoCloudMLVBDev/usersig_server_source/tree/master/java)|
| php | `genUserSig` | [Github](https://github.com/TencentVideoCloudMLVBDev/usersig_server_source/tree/master/php)|
| nodejs | `genUserSig` | [Github](https://github.com/TencentVideoCloudMLVBDev/usersig_server_source/tree/master/nodejs)|







