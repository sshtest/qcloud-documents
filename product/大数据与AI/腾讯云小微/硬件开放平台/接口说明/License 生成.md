
## 概念说明

### PID(product id：产品 ID)
**PID** 是小微平台对一款产品的唯一识别标识。

对于合作方而言，您旗下的一款产品可以申请一个独立的 **PID**，小微会为之分配独立的配置信息。

以音箱类设备为例，假设您旗下有两款产品，一款支持 QQ 通话，而另一款性能较差无法支持，就可以通过创建两个的 **PID**，在设备配置平台进行不同的 skills 配置。

### SN(serial number：序列号)

每台设备都应该有一个唯一的序列号，以区分身份。

序列号并不需要小微平台分配，所以您可以直接对接原有产品线的序列号系统。

需要注意的是，我们对序列号格式有严格要求：长度必须是 16 个字符的字母、数字或者连词符的组合，例如 ABC-0032-1234567 就是一个非常标准的序列号。

您也可以使用小微提供的 [客户端工具](http://qzonestyle.gtimg.cn/qzone/vas/opensns/res/doc/key_tools_v3.01.zip)，来生成符合要求的序列号。

### license(SN 对应的认证签名)

**license** 用于安全校验，作用等同与密码。

**license** 的存在意义和目的，是保护合作方的利益，避免冒充合作方的山寨设备出现，**license** 可以使用 [客户端工具](http://qzonestyle.gtimg.cn/qzone/vas/opensns/res/doc/key_tools_v3.01.zip) 来实现。

### 签名密钥
在对接小微的过程中，会遇到两对非对称密钥，我们需要用它们来生成 **license**，以保证设备的合法性验证。

通过密钥，我们可以确定是否有其它厂商冒用您公司的身份接入小微平台，我们称之为签名密钥。

既然是非对称密钥，签名密钥也一定是成对存在的，分为一个 ** 签名公钥 ** 和 ** 签名私钥 **：

> *   ** 签名私钥 ** 用来根据 **SN** 生成 **license**，千万不可以泄露，否则就起不到签名的作用了。
> *   ** 签名公钥 ** 则需要上传到小微平台，用来验证设备的合法性。

### 通道密钥

上文提到的另一对密钥则是用来进行安全加密的，以确保整个过程中的通讯信息只有小微可以解密，我们称之为 ** 通道密钥 **（也叫做 ** 服务器密钥 **） 。

这对密钥是创建新的 **PID** 时，小微平台自动生成的。

> *   ** 通道私钥 ** 会保存的小微平台，在对接过程中是不可见的。
> *   ** 通道公钥 ** 可以从配置平台的页面上直接下载，其本质就是一个文本文件，具体用法可以参考生成说明。

## 生成说明

### 获取通道公钥

在小微硬件平台提交新的产品信息后，会在该产品的配置页面上看到 **PID** 和与之对应的 ** 通道公钥 **（** 服务器公钥 **），下载即可。

> *   页面上下载到的 ** 通道公钥 ** 是以 $PID$.pem 的方式命名的，如：2100049457.pem。

### 生成签名密钥

*   首先下载 [客户端工具](http://qzonestyle.gtimg.cn/qzone/vas/opensns/res/doc/key_tools_v3.01.zip)，然后运行之。（目前工具只支持 windows 版本）

*   在工具中点击 ** 生成 KEY** 按钮，便会在指定的目录下生成两个密钥文件：

    一个叫做 ec_key.pem，也就是需要妥善保管的 ** 签名私钥 **，它将会在后面生成 **license** 时派上用场。

    另一个叫做 public.pem 的就是签名公钥，这个文件是产品配置过程中的必备信息，需要在配置页面上进行上传。

*  回到产品配置页面，点击 ** 公钥上传 ** 按钮，选择刚才生成的 public.pem，就可以完成此步骤了。

### 在代码中使用设备信息

 可以参照 Demo，更新设备端代码中的 **PID**、** 通道公钥 **(puKey/publicKey)、**SN**。

 **license** 也是需要生成 & 更新的，对于 **license** 的生成有以下两个推荐方案。

### license 生成 - 批量方案

可以使用 [客户端工具](http://qzonestyle.gtimg.cn/qzone/vas/opensns/res/doc/key_tools_v3.01.zip) 或 [工具源代码](http://qzonestyle.gtimg.cn/qzone/vas/opensns/res/doc/KeySnLicense_v1.0.tar.bz2)，根据 **SN** 批量生成 **license**，然后在设备出厂前，内置到设备中。

使用 ** 客户端工具 ** 生成 license 的步骤如下：

*   首先，选择 ** 使用自有序列号 (SN)**，批量导入 **SN**，您也可以使用 ** 客户端工具 ** 随机生成一批 SN 来使用。

    [![img](http://qzonestyle.gtimg.cn/qzone/vas/opensns/res/img/ipcamera_9.png)](http://qzonestyle.gtimg.cn/qzone/vas/opensns/res/img/ipcamera_9.png)

> *   注意：随机生成 **SN** 是针对当前运行而言的，如果之前已经存在一批 SN，** 客户端工具 ** 无法保证是否会和之前的 SN 重复。

*   <div class="md-text"> 有了 **SN**，我们就可以在 ** 证书生成工具 ** 区域，生成 license 了。生成前，需要选择要使用的 ** 签名私钥 **，然后点击 ** 生成 License** 即可。</div>

    <div class="col-sm-12">[![img](http://qzonestyle.gtimg.cn/qzone/vas/opensns/res/img/ipcamera_10.png)](http://qzonestyle.gtimg.cn/qzone/vas/opensns/res/img/ipcamera_10.png)</div>

> *   注意：license 的生成使用的是 ** 签名密钥 ** 对中的私钥，文件名叫做 ec_key.pem，需要和上传到小微平台的 ** 签名公钥 ** 匹配。

### license 生成 - 运行时方案

如果 **license 生成 - 批量方案 ** 对于您的生产流程会产生比较大的影响，也可以只在设备中内置 **SN**，在设备首次运行（或重置后首次运行）时，再根据 ** 签名私钥 ** 生成 **license**。

但是，这里需要务必注意的是：我们 ** 强烈不建议 ** 把 ** 签名私钥 ** 放在设备本地，因为一旦泄露，第三方即可生产相同身份的设备，您产品的权益将难以得到保证。

所以，建议把 ** 签名私钥 ** 放在云端，并设计一套安全的通信方式，来根据 **SN** 换取 **license**。

> *   设备端 Demo 之所以把私钥放在云端，是为了方便对接小微平台的开发人员理解 **license** 的生成过程。

## 源码 & 工具获取
[客户端工具](http://qzonestyle.gtimg.cn/qzone/vas/opensns/res/doc/key_tools_v3.01.zip)

[工具源代码](http://qzonestyle.gtimg.cn/qzone/vas/opensns/res/doc/KeySnLicense_v1.0.tar.bz2)
