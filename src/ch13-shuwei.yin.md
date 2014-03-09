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

ng-view direcitve是一个特殊的directive，它包含在ngRouter模块，其具体职责是作为一个占位符在$route视图内容区中。

在嵌套的模板里它创建自己的scope。

*注：ng-view directive是第1000优先级的最后一个。Angular不会在一个较低优先级的元素上运行任何directive(即在&lt;div ng-view>&lt;div>元素里的其他direcitve是没有任意义的)*

ngView directive具体步骤如下：

* 任何时候当$routeChangeSuccess事件触发时，视图将更新。
* 如果当前路由有相关联的模板
	* 创建一个新scope
	* 移除上一个视图，清除其scope
	* 链接新的scope和新的模板
	* 如果路由中指定controller，将其链接到scope
	* 发出$viewContentLoader事件
	* 运行onload函数（如果提供）
	
###Routes（路由）

在AngularJS中定义所有应用程序的路由有两种方法，我们先用其中一种：when方法、otherwise方法。

为应用的特定module创建一个路由，可以用config函数。

angular.module('myApp', []). config(['$routeProvider', function($routeProvider) { }]);

*在这里我们使用特殊的依赖注入的语法，关于依赖注入的更多信息，请查看 依赖注入 章节*

现在，我们用when方法增加了一个特定的路由，这个方法接收两个参数（when(path, route))）。

下面说明如何创建一个单一的路由：

angular.module('myApp', []). config(['$routeProvider', function($routeProvider) {    $routeProvider      .when('/', {        templateUrl: 'views/home.html',        controller: 'HomeController'      });}]);
第一个参数是路由的路径，匹配$location.path，是当前的URL。末尾或双斜杠仍然工作。我们可以在URL的冒号后面带参数（如，:name）。后面会讲到如何用$routeParams取出这些参数。
第二个参数是configuration对象，如果路由的第一个参数匹配，决定到底要做什么。configuration对象的属性可以设置controller，template，templateURL，resolve，redirectTo，reloadOnSearch。

更复杂的路由场景需要多个路由和全方位的重定向路径。

angular.module('myApp', []). config(['$routeProvider', function($routeProvider) {    $routeProvider      .when('/', {        templateUrl: 'views/home.html',        controller: 'HomeController'      })      .when('/login', {        templateUrl: 'views/login.html',        controller: 'LoginController'      })      .when('/dashboard', {        templateUrl: 'views/dashboard.html',        controller: 'DashboardController',        resolve: {user: function(SessionService) {return SessionService.getCurrentUser();} }      })      .otherwise({        redirectTo: '/'      });}]);

###controller

controller: 'MyController'// orcontroller: function($scope) {}
如果我们在configuration对象中设置controller属性，它将与该路由的新作用域相关联。如果我们传递一个字符串，它会把module中已注册的controller和新的路由关联在一起。如果我们传递一个函数，该函数将模板中的DOM元素当做作controller与其相关联。

###template

template: '<div><h2>Route</h2></div>'。

如果我们在configuration对象中设置了controller属性，Angular将在ng-viewDOM节点中渲染此段HTML模板。

###templateUrl

templateUrl: 'views/template_name.html'

如果设置了templateUrl属性，Angular应用将试图通过XHR（使用$templateCache）取得视图。如果找到模板便装载此模板，Angular将在ng-view DOM元素中渲染模板内容。

###resolve

resolve: {'data': ['$http', function($http) {return $http.get('/api').then(function success(resp) { return response.data; } function error(reason) { return false; }); }]}

如果我们设置了resolve属性，Angular将注入map元素到controller中。这些依赖都是promises，它们将在controller装载及$routeChangeSuccess事件触发之前被转变为设置的值。

map对象可以是：
	* key：key作为依赖的字符串将注入到controller中。
	* factory：可以是service的名字，也可以是函数的返回值，被注入到controller中，或者是已转变后的promise（返回的结果注入到controller中）
	
在上面的例子中，resolve发送$http请求后，将请求后的返回值填 充给'data'。map对象的key（data）被注入到controller中，它将在controller中取到。

###redirectTo

redirectTo: '/home'
//or
redirectTo: function(route, path, search)

如果设置redirectTo为字符串，那么该值将改变路径，并触发路由转换到新的路径。

如果设置redirectTo为一个函数，该函数的返回值将被设置为新路径的值，并触发route-change请求。该函数接受以下参数：

1：从当前路径提取的路由参数
2：当前的路径
3：当前的search部分

###reloadOnSearch

如果reloadOnSearch属性设置为true（默认），当$location.search（）改变时重新装载route。如果将其设置false，当URL的search部分改变时将不重新装载route。小窃门：可以用在嵌套的路由或页码改变的场景。

现在我们用when函数创建路由。

在这个例子中，我们将创建两个路由：根路径和inbox路径。我们也可以设置主路径为默认路径。

angular.module('MyApp', []). config(['$routeProvider', function($routeProvider) {    $routeProvider      .when('/', {        controller: 'HomeController',        templateUrl: 'views/home.html'      })      .when('/inbox/:name', {        controller: 'InboxController',        templateUrl: 'views/inbox.html'})      .otherwise({redirectTo: '/'});  }]);
  
上面的例子，我们用when方法创建了三个路由。当没有匹配的路径时，otherwise方法被调用。通过otherwise方法，我们设置了“/”为默认路径。

当浏览器加载Angular应用时，会默认设置为缺省路由的URL。默认是“/”，除非浏览器加载时使用不同的URL。

###$routeParams

前文提到，如果我们用冒号启动路径参数，AngularJS将解析出来，并把它传递给$routeParams。例如，我们创建一个象这样的路由：
$routeProvider  .when('/inbox/:name', {    controller: 'InboxController',    templateUrl: 'views/inbox.html'  })
Angular将填入key为:name的对象给$routeParams，其值将填入被加载的URL的值。
例如浏览器加载的URL为 /inbox/all，$routeParam对象将是这样：
{name: 'all'}
提示：在controller注入$routeProvider，能访问到这些变量：
app.controller('InboxController', function($scope, $routeParams) { 
	// We now have access to the $routeParams here});
###$location Service
AngularJS提供了解析地址栏中的URL，同时允许您访问应用程序的当前路径的路由。它还使您能够改变路径，处理任何类型的导航。
*$location服务为JavaScript的window.location对象提供了更好的接口，并直接集成了我们的AngualrJS应用程序。*

每当我们需要为我们的应用提供一个内部重定向时，将使用$location服务，
如用户注册后、更改设置，或登录后跳转。$location服务不能刷新整个页面。如果要刷新整页，需要用$window.location对象（window.location的接口）。
###path（）
获取当前的路径，可以使用path(）方法：
$location.path(); // 返回当前path（路径）
改变当前路径，跳转到另一个URL：
$location.path('/) //改变路径到'/'路由
path方法与HTML5的history API相互作用，所以用户能按回退按钮返回上一个页面。###replace()
如果你想完全重定向，不给用户使用回退按钮返回的能力（跳转后再重定向，如登录后的跳转），AngularJS提供replace方法：
$location.path('/home'); $location.replace();// or 
$location.path('/home').replace();###absUrl()如果您想获得表示所有片段的完整URL，可以使用absUrl()方法：$location.absUrl()
###hash()