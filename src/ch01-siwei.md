# 简介
### 前言

我现在对那些平时发表的Javascript库和框架已经变得有点麻木了.然而拥有从各种库与框架中去选择的能力是一件好事，在应用中包含太多的脚本不利于后期维护，至少我是这样认为的.我一直关注的是由于越来越多的脚本加入到一个应用中所产生的依赖性，并且希望一两个脚本就能提供我想要的主要功能。

当我第一次接触AngularJs时我就被它吸引了,因为我感觉它能提供一个独立的框架用来构建各种动态的、用户体验很好的应用.在我了解更多信息以后,我确认了我的第一印象。AngularJS具有一系列强大的特性，它提供了一种模块化编写代码的方法,这种模块化编程的方法有利于复用、后期维护以及测试。AngularJS的主要特性包括,支持DOM操作、动画、模板、数据的双向绑定，路由、history、ajax以及单元测试等等。

 虽然有一个核心框架作为基础是非常好的，但是学习这些框架还是很具有挑战性的。当我深入学习AngularJS时，我被各种概念弄得非常困惑，甚至怀疑它是否真的适合我。什么是service?它与factory有什么不同?scope是怎样注入的？directive又是什么?我首先要做的就是把这些零碎的知识点弄懂然后再全面整体的去了解.如果有一些资源去参阅，学习这些东西就会容易很多.
 
 幸运的是，在ng-book中有很好的资源供你使用：The complete Book on AngularJS 能帮你立刻有所收获。Ari Lerner 将自己所学的专业知识以一种容易理解的方式
呈现出来。如果你想学习更多关于数据绑定，“live"模板如何工作，测试AngularJS应用的程序的知识，那么你选择对了。AngularJS是一款非常强大和有趣的框架。
此书中的例子将会帮助你在框架方面迅速提高。祝你们在AngularJS开发项目中好运！
Dan Wahlin Wahlin 参阅<http://weblogs.asp.net/dwahlin1>与<http://twitter.com/DanWahlin2>

###致谢
首先，我想感谢一直以来鼓励我写这本书的每个人。说出书很容易的人都是从未自己写过书的人，我想感谢Q Kuhns的不懈努力，在语法上的编辑和对我的支持。感谢Erik Trom的耐心和对细节的关注，感谢Nate Murray 的乐观和清晰的思维。特别要感谢Hack Reactor的全体成员和2013年暑期给我提供研究如何有条理的教学AngularJS的空间。我也要感谢我的30x500校友，Sean Iams,Michael Fairchild,Bradly Green,Misko Hevery和AirPair团队。最后，我要感谢我们此书发行前期给过帮助的人，我们得到了业界极大的支持和帮助，再此特别感谢Philip Westwell,Saurabh Agrawal,Dougal MacPherson

###关于作者
Ari Lerner是fullstack.io的联合创始人，Fullstack建立在加州的旧金山。他曾在AT&T位于加州的帕洛阿尔托创新中心工作了五年，建立了庞大的云基础并帮助设计前沿开发者中心，包括设计开放的APIs和开发者工具箱.他和他的团队使公司工作流程和内部程序现代化，这让他们在AT&T 2012年度报告中表现突出。他从AT&T离职继续从事建立Fullstack.io，一个全栈的软件开放产品，同时经营专注于从硬件到浏览器的全部堆栈的服务公司.他与美丽的女友和一只可爱的狗居住在旧金山。

###关于本书
ng-book:The Complete Book on AngularJS 为你提供了成为AngularJS高手的解决方案。AngularJS是Google的一个团队推出的一项前端框架技术。它能让你快速简单的建立丰富的前端体验。
ng-book:The Complete Book on AngularJS 提供了一些你需要了解AngularJS并迅速创立印象深刻的网页体验的前沿工具，本书提出了挑战，也提供了实在的技术供你在网页应用中迅速使用。
本书涵盖了很多知识点，能够帮助你建立运行完美的专业网页APPS：

- 与RESTful网页服务互动
- 建立用户可重复利用的组件
- 测试
- 异步编程
- 建立服务
- 提供高级可视化


此书的目的是不仅让读者清楚的了就AngularJS如何工作，还提供一些专业代码片段以供读者根据自己的应用建立和修改。
利用这些工具盒测试，你可以利用AngularJS钻研建立自己的动态网络应用，并且是可以扩展的。


###关于读者

此书写给那些从未使用过AngularJS、建立网页应用但是对JavaScript框架的使用很好奇的读者。你们或许具备HTML和CSS的知识，并且熟悉基础的JavaScrip(和其他的JavaScript框架）

###此书机构
此书涵盖基础知识，旨在让读者看很快利用AngularJS写动态网页应用。
然后我们会讲解AngularJS如何工作，它与普通的JavaScript网页框架有什么不同。我们还会深入探讨AngularJS应用流程的基础的细节。最后，用我们的了解的所有知识建立相关的庞大的应用，
###其他资源
我们参考 了AngularJS网页的官方资料。官方的AngularJS文档是很大的资源，我们会经常用到。我们建议读者阅读下AngularJS API 文档，读者可以直接了解写AngularJS 应用的推荐方法。当然，它也提供最新的可用文档。

###书中的表达约定

在整本书中你将看到以下表达约定，代表不同信息：
内嵌代码引用：&lt;h1>Hello&lt;/h1>

代码块：
<pre>
<code>
var App = angular.module('App', []);
function FirstController($scope) {
	$scope.data = "Hello";
}
</code>
</pre>

命令行的任何命令：
$ ls -la

在Chrome(我们主要开发用到的浏览器)的开发者控制台的任何命令：

 &gt; var obj = {message: "hello"};


重要词汇将用黑体字表示

技巧提示将显示如下：

Tip
这是提示信息

警告提示如下：
这是警告信息


错误信息显示如下：
Error
这是错误 信息

重要的插图编号显示如下：
INFO
消息盒

讨论话题显示如下：
Discussion
这是讨论话框


###开发环境

为了使用AngularJS写任何应用，首先我们需要一个良好的开发环境。在整本书中，我们的大部分时间将会花在两个地方：我们的文本编辑器和浏览器。

我们推荐将文本编辑器作为你的编辑器，浏览器作为编写的浏览器。使用本书我们强烈推荐读者使用谷歌Chrome浏览器，作为开发工具它提供了最好的开发环境。

我们只需要安装几个数据库,只需要karma和nodejs来进行测试。虽然不严格要求，但最好能够安装git。本书不会讲解如何安装NodeJS,请访问nodejs.org获取更多信息。虽然我们的大部分工作会在浏览器中完成，本书的部分内容会讲到建立RESTful APIs服务于我们的前端数据端点。

 
