# 创建Agular的Chrome应用

Chrome浏览器由Google定制。它不仅速度极快，走在web开发的前沿，并且在离线和在线方面传达了最好的网络体验。

Chrome应用就是把应用程序嵌入到网页浏览器中运行，但旨在表现出本地应用的感受。由于在Chrome中运行，它们用HTML5、JavaScript和CSS3编写，能够达到和本地应用一样的性能，而普通网页应用是达不到的。

Chrome应用利用Chrome提供的API和服务，能够给用户带来完美的桌面体验。

另一个值得关注的差异是：Chrome应用通常从本地加载数据，所以展现快速及时；网页应用则需要等到页面元素从网络加载完成才能进行渲染。当我们运行应用时，这一特点大幅提升了应用的性能和用户体验。

## 理解Chrome应用

我们来深入研究一下Chrome应用实际上是如何工作的以及我们如何开始创建自己的Chrome应用。每一个Chrome应用都包含三类核心文件：

### manifest.json

manifest.json文件描述了与应用有关的元数据，比如：*名称、描述信息、版本号*。该文件也描述了启动应用的方法。

### 后台脚本文件

后台脚本设置了应用如何响应系统级事件，比如用户正在安装应用或正在启动应用，等等。

### 视图文件

大多数Chrome应用都包含一个视图。虽然视图不是必需的，但被普遍使用在应用程序里。


## 创建Chrome应用

这一节，我们将使用Angular轻松创建一个高级Chrome应用。我们将模拟创建一个有意思的名叫*[Currently](https://chrome.google.com/webstore/detail/currently/ojhmphdkpgbibohbnpbfiefkgieacjmh)*的Chrome网页应用，作者团队来自*[Rainfall](http://blog.rainfalldesign.com/)*。

![Currently](此处有图片，见原版)

我们给即将模拟创建的应用叫做*Presently*吧。

###构建Presently

创建Presently之前，我们需要考虑应用程序的结构。这样做的话，在开始编码的时候，有利于开拓我们创建应用的思路。

像*Currently*一样，*Presently*也是一个“newtab”应用，意味着我们每次打开一个新的标签页的时候，它都会运行。

*Presently*有两个主界面：

* 首屏界面

这一屏的特点是展示了当前的时间和天气。在天气的旁边还标注了一些天气图标。

* 设置界面

这一屏允许用户在应用中改变他们的地点定位。

为了实现首屏界面的功能，我们需要给定一个恰当格式的日期和时间，并且通过远程API服务获取到天气数据。

要实现设置界面的功能，我们必须利用远程API服务把自动建议位置的功能整合到一个输入框里。

最后，我们使用基础的本地存储（会话存储）功能将用户设置保存在应用中。

##搭建骨架

我们将建立一个这样的文件结构来创建我们的应用：

![文件结构](此处有图片，见原版)

我们把CSS文件放在*css/*目录下，把自定义字体放在*font/*目录下，把JavaScript文件放在*js/*目录下。我们将*js/app.js*文件设为JavaScript主文件，把应用的HTML内容放在根目录的*tab.html*文件中。

![tips](此处有图片，见原版)有些不错的工具可以帮助启动Chrome应用扩展程序，比如*[Yeoman](http://yeoman.io/)*。