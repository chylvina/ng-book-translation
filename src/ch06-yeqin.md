##控制器（p25-p30）

在AngularJS中，控制器是用来增强AngularJS应用的视图层。在Hello world示例应用里，我们只使用了一个隐式控制器，没有明确定义一个控制器。
AngularJS中的控制器是一个函数，它用来给视图层作用域增加额外的功能。我们用控制器来设置作用域对象的初始状态，添加自定义行为。
当在页面中创建了一个控制器，Angular传递给控制器一个新的$scope参数，通过这个新的$scope，我们可以设置控制器的作用域的初始状态。
由于AngularJS为我们提供了对控制器的处理，我们只需要编写控制器构造函数即可。

编写一个控制器，初始化作用域如下：<pre><code>function FirstController($scope) {	$scope.message = "hello";}
</code></pre>
*关于控制器编写的一个好习惯是使用[Name]Controller而不是[Name]Ctrl方式来命名。*
我们可以看到，Angular在创建作用域的时候会调用控制器。

细心的读者会注意到我们在全局作用域下创建了函数。这样做通常不是一个好的方式，因为它污染了全局命名空间。更合理的创建方式是创建一个模块，然后在模块中创建控制器，比如像下面的例子：<pre><code>var app = angular.module('app', []); 
app.controller('FirstController', function($scope) {  $scope.message = "hello";});
</code></pre>
为了在视图层中创建自定义行为，我们可以直接在控制器的作用域上创建函数。幸运地是，AngularJS允许我们的视图调用在$scope上的函数，就如同我们在$scope上获取数据。

我们使用一个内置的指令ngclick绑定在按钮，链接（或者任意一种DOM元素）上。ng-click指令将DOM元素click事件处理函数绑定到浏览器click过程的mouseup事件中（当浏览器触发DOM元素click事件时，处理函数将会被调用），同前面的例子类似，绑定过程的示例代码如下所示：

<pre><code>
<div ng-controller="FirstController"><h4>The simplest adding machine ever</h4><button ng-click="add(1)" class="button">Add</button><a ng-click="subtract(1)" class="button alert">Subtract</a> <h4>Current count: {{ counter }}</h4></div>
</code></pre>按钮和链接都绑定了$scope上的一个处理函数，当他们被按下（被点击）时，Angular将调用处理函数。
注意当我们告知Angular什么方法被调用时，我们把它连同括号放到一个字符串中（add(1)）。

现在，让我们在FirstController中添加一组函数。
<pre><code>
app.controller('FirstController', function($scope) { 
	$scope.counter = 0;	$scope.add = function(amount) { 		$scope.counter += amount; 
	}; 
	$scope.subtract = function(amount) { 
		$scope.counter -= amount; 
	};});
</code></pre>
通过这种方式设置的FirstController可以让我们调用定义在FirstController作用域或包含其作用域的父$scope中的add或substract函数（如上所示）。

使用控制器能够让我们在单个容器中包含单一视图的逻辑。
一个好的编程习惯是保持控制器简洁。
AngularJs开发者通过Angular的依赖注入特性来获取服务，从而保持控制器简洁。
同其他Javascript框架的一个重要区别是，AngularJS仅作为视图层和$scope模型层之间的一种连接。
除了存储模型数据之外，控制器并不适合用来进行DOM操作,DOM格式化，数据操作，或是状态维护。

AngularJS能够在$scope上设置任何类型，包括对象，并能将对象属性展示在视图层。

例如，我们可以在控制器MyController中，创建一个只包含name属性的person对象：<pre><code>app.controller('MyController', function($scope) { $scope.person = {    name: "Ari Lerner"  };});
</code></pre>

在ng-controller='MyController'所在的div中的任何子元素中，我们都能够得到$scope上的person对象。例如，现在我们能够在视图中得到person对象的引用以及获取到person.name。
<pre>	<div ng-app="myApp">		<div ng-controller="MyController">			<h1>{{ person }}</h1>			and their name:			<h2>{{ person.name }}</h2>  		</div>	</div></pre>
我们可以看出$scope对象怎样从模型层到视图层传递信息。
我们也可以用同样的方式来监听事件，同应用的其他部分进行交互，或是创建应用的特定逻辑。

Angular 使用作用域来分离视图层，控制器和指令（本书后面将介绍这个概念）各自职能，这也使得每块特定功能的测试编写变得简单。

###控制器层级（作用域嵌套）
AngularJS应用中每个部分都有一个父作用域（在ng-app层级上，这个作用域叫$rootScope）,这与其在哪个上下文中渲染无关。

*这里有一个例外：指令中创建的作用域是独立的作用域。*

除了独立的作用域，被创建的作用域都遵循原型继承规则，即他们能够访问到父级作用域。如果我们了解面向对象编程，这个行为看起来很熟悉。

默认情况下，AngularJS如果在当前作用域下查找不到某个属性，它会向上到其父作用域中查找，若仍找不到会继续向上查找，以此类推直到到达$rootScope作用域。如果在$rootScope中还没有找到，它将不会更新视图层，然后继续执行其他部分。
为了理解上面的过程，我们创建一个ParentController，一个ChildController。ParentController中定义一个person对象。ChildController包含在ParentController中，它引用了person对象：<pre><code>app.controller('ParentController', function($scope) { 
	$scope.person = {greeted: false};});app.controller('ChildController', function($scope) { 
	$scope.sayHello = function() {    	$scope.person.name = "Ari Lerner";		$scope.person.greeted = true; 
	}});
</code></pre>
如果我们在视图层中ParentController里嵌入了ChildController，那么ChildContrler中$scope的父对象就是ParentController的$scope对象。
通过原型继承行为，我们可以在子作用域中引用到ParentController包含的$scope中的数据。

例如我们可以在ChildController中引用定义在ParentController里的person对象。
<pre><code><div ng-controller="ParentController"> 
	<div ng-controller="ChildController">		<a ng-click="sayHello()">Say hello</a> 
	</div>	{{ person }}</div>
</code></pre>

控制器的这种层级结构非常类似DOM本身的结构。

我们可以看到，当按下button按钮时，我们在ChildController里获取到了ParentController作用域的$scope.person，就像person是在ChildController的$scope中定义的一样。
一个好的编程习惯是尽量保持我们的控制器简洁，而与之相对的坏的编程习惯是把DOM交互，或数据操作逻辑放在控制器中。

例如，下面的控制器代码过于繁琐，它包含了DOM处理和很多视图层的处理逻辑：
<pre><code>
angular.module('MyController', function($scope) { $scope.shouldShowLogin = true; $scope.showLogin = function() {    $scope.shouldShowLogin = !$scope.shouldShowLogin;   }$scope.clickButton = function() { $("#btn span").html("Clicked");}$scope.onLogin = function(user) {    $http({      method: 'POST',      url: '/login',      data: {user: user }}).success(function(data) { // user}) }})
</code></pre>一个良好设计的应用会使用指令和服务来处理逻辑。我们将前面的控制器用指令和服务来重新改写，可以得到一个更简洁，更易于维护的控制器：
<pre><code>
angular.module('MyController', function($scope, UserSrv) { 
	// The content can be controlled by	// directives	$scope.onLogin = function(user) {    	UserSrv.runLogin(user);  	}})
</code></pre>