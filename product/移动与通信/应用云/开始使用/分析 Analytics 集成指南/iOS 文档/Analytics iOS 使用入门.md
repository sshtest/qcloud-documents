
#应用云 Analytics 服务 iOS 集成指南


## 准备工作

为了使用应用云 iOS 版本的 SDK ，您首先需要做好以下工作：

有一个 iOS 的工程，或者下载我们的 [实例工程](https://console.cloud.tencent.com/tac)。

## 集成代码库

### 通过 CocoaPods 集成 (**推荐**)

如果您是设置一个新项目，则需要安装 SDK ，您可能已经在创建应用云项目的过程中完成此步操作。

我们 **强烈建议** 使用 CocoaPods 来安装相关的库。这样可以方便您后期 **维护** 和 **即时** 收到我们的 **SDK 更新** 。您可以根据 [安装说明](https://guides.cocoapods.org/using/getting-started.html#getting-started) 来安装并使用 CocoaPods 。如果您不想使用 CocoaPods ，则可以按照 **手工集成** 的方式直接集成 SDK 框架。

如果您计划下载并运行某个快速入门实例，实例中会提供 Xcode 项目和 Podfile 。不过您还是需要安装 Pod 并下载 tac_services_configurations.json 文件。如果您希望将应用云库集成至自己的某个项目中，则需要为想要使用的库添加 Pod。


#### 为新项目添加 应用云  iOS 库

##### 1. 如果没有 Xcode 项目，请新建一个。
##### 2. 如果还没有 Podfile，请创建一个。

~~~
$ cd your-project directory
$ pod init
~~~

并在您的 Podfile 文件中添加应用云的私有源：

~~~
source "https://git.cloud.tencent.com/qcloud_u/cocopoads-repo"
source "https://github.com/CocoaPods/Specs"
~~~

> **注意：**
> 一定要添加 [CocoaPods](https://github.com/CocoaPods/Specs) 的原始源，否则会造成部分仓库找不到的问题。

##### 3. 添加您想安装的 Pod。您可以按照以下方法在 Podfile 中纳入一个 Pod。

~~~
pod 'TACCore'
~~~


这会在您的 iOS 应用中添加 应用云 正常运行所需的必备库以及 Analytics for 应用云 功能。下面列出了目前可供使用的一系列 pod 和 subspec，在针对不同功能的设置指南中也对此给出了相应的链接。

##### 4. 安装 Pod 并打开 .xcworkspace 文件以便在 Xcode 中查看该项目。

~~~
$ pod install
$ open your-project.xcworkspace
~~~

##### 5. 从应用云控制台中下载一个 tac_services_configurations.json 文件并将其添加到您的应用中。

### 手工集成

### 在您的应用中初始化应用云

最后一步是向您的应用添加初始化代码。您可能已经在将应用云添加到应用时完成了此步骤。如果您使用的是快速入门示例，则此步骤已替您完成了。

#### 步骤 1 在 UIApplicationDelegate 子类中导入应用云模块。
Objective-C 代码示例：
~~~
import <TACCore/TACCore.h>
~~~

Swift 代码示例：
~~~
import TACCore
~~~


#### 步骤 2 配置一个 TACApplication  共享实例，通常是在应用的 application:didFinishLaunchingWithOptions: 方法中配置。


##### 使用默认配置，通常对于 应用云 的项目他的配置信息都是通过读取 tac_services_configurations.json 文件来获取的：
Objective-C 代码示例：
~~~
    [TACApplication configurate];
~~~
Swift 代码示例：
~~~
	TACApplication.configurate();
~~~
##### 需要通过编程的方式自定义某些参数：

通常对于应用云的项目它的配置信息都是通过读取 tac_services_configurations.json 文件来获取的。但是，您可能也有需求在程序运行时，去改变一些特定的参数来改变程序的行为。为了支持您的这种需求，我们增加了修改程序配置的接口，您可以仿照如下形式来修改应用云的配置。

Objective-C 代码示例：
~~~
    TACApplicationOptions* options = [TACApplicationOptions defaultApplicationOptions];
	// 自定义配置
	// opions.xxx= xxx ;
    //options.analyticsOptions = xxx;
    [TACApplication configurateWithOptions:options];
~~~
Swift 代码示例：
~~~
	let options = TACApplicationOptions.default()
	// 自定义配置
	// opions.xxx= xxx;
  // options.analyticsOptions = xxx;
	TACApplication.configurate(with: options);
~~~
