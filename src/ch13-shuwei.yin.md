## 多视图和路由
在单页应用中，从一个页面跳转到另一个页面是至关重要的。当应用越来越复杂时，我们需要一种方法来管理页面，使用户看到他们导航的应用程序路径。

我们已经支持在主HTML中包含模板代码，但这样管理不仅仅是嵌入的代码不断增长和难于维护，也很难使其他开发者参与开发。

当然在视图中包含更多的模板（可以用ng-include directive），可以打破布局视图和模板视图，只显示当前用户访问的URL视图。

我们可以将那些分解的片段模板组合在布局模板中。AngularJS允许我们用$route服务的提供者$routeProvider定义路由。

使用$routeProvider，能获得浏览者的历史导航记录，使浏览器当前的URL地址的用户书签可用、共享特定的页面。

###安装
在1.2.2版本中，ngRoutes从Angular的核心模块中拆出来。如果要在Angular应用中使用routers，需要安装并引用进我们的应用中。

从code.angularjs.org（链接：http://code.angularjs.org）下载，保存在本地，在HTML中 js/verdor/angular-route.js 这样引用。

也可以利用Bower安装，通常保存在Bower的目录下。关于Bower的更多信息，请参考Bower章节.

$ bower install --save angular-route

在HTML中引用Angular文件后需要引用angular-router文件。

&lt;script src="js/vendor/angular.js">&lt;/script>

&lt;script src="js/vendor/angular-route.js">&lt;/script>

最后，我们需要在app模块中依赖引用ngRouter模块。

angular.module('myApp', ['ngRoute']);

###布局模板
制作一个布局模板，需要更改HTML，以便告诉AngularJS在哪里渲染模板。在路由组合中使用ng-view directived，我们可以精确的指定在DOM树中的哪里放置当前路由要渲染的模板。


例如，一个布局模板可能看起来象这样(代码见137p &lt;header>部分)：

在这个例子中，我们可以替换所有渲染的内容到 &lt;div class="content">中，而我们也可以在路由变化时保留完整的&lt;header>和&lt;footer>元素。

ng-view direcitve是特殊的directive，它包含ngRouter模块，


