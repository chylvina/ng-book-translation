##控制器（p25-p30）

在AngularJS中，控制器的作用是用来增强AngularJS应用的视图层。在Hello world示例应用里，我们只使用了一个隐式控制器，没有明确定义一个控制器。AngularJS中的控制器是一个函数，它用来给视图层作用域增加额外的功能。我们用控制器来设置作用域对象的初始状态，添加自定义行为。当我们在页面中创建了一个控制器，Angular传递给控制器一个$scope参数，通过这个$scope参数，我们可以在控制器中设置作用域的初始状态。
由于AngularJS为我们提供了对控制器的处理，我们进需要编写控制器构造函数即可。
编写一个初始化控制器如下：<pre><code>function FirstController($scope) {	$scope.message = "hello";}
</code></pre>
*控制器一个好的编程习惯是通过[Name]Controller命名，而不是[Name]Ctrl命名。*
我们可以看到，Angular在创建作用域的时候会调用控制器。
细心的读者会注意到我们在全局作用域下创建了函数。
这样做通常不是一个好的方式，因为我们不想污染全局命名空间。更合理的创建构造器方式是，我们创建一个模块，然后在模块中创建控制器，比如像下面的例子：<pre><code>var app = angular.module('app', []); 
app.controller('FirstController', function($scope) {  $scope.message = "hello";});
</code></pre>
为了在我们的视图层中创建自定义行为，我们简单在控制器的作用域上创建了函数。

幸运地是，AngularJS 允许我们视图层调用在$scope上的函数，就想我们获取数据一样。
为了绑定按钮，链接（或者任意一种DOM元素），我们需要使用另外一个内置的指令，ngclick。
 ng-click指令将DOM元素click事件处理函数绑定到浏览器click过程中的mouseup事件中（如当DOM元素事件触发时，事件处理函数将会被调用），示例代码如下：

<pre><code>
<div ng-controller="FirstController"><h4>The simplest adding machine ever</h4><button ng-click="add(1)" class="button">Add</button><a ng-click="subtract(1)" class="button alert">Subtract</a> <h4>Current count: {{ counter }}</h4></div>
</code></pre>按钮和链接都绑定在$scope上的一个处理函数，当他们被触发（被点击）时，Angular将调用处理函数。
注意当我们告知Angular什么方案被调用时，我们使用了把他放在一个包含括号的字符串中（add(1)）。
现在，我们在FirstController中创建行为。
<pre><code>
app.controller('FirstController', function($scope) { 
	$scope.counter = 0;	$scope.add = function(amount) { 		$scope.counter += amount; 
	}; 
	$scope.subtract = function(amount) { 
		$scope.counter -= amount; 
	};});
</code></pre>
这种方式设置我们的FirstController让我们可以调用在FirstController作用域中或在包含$scope父作用域中定义的add或substract函数（如上所示）。
使用控制器能够让我们在包含逻辑在一个容器中的视图层。一个好的实践是保持控制器精简。作为AngularJs开发者能够做的一种方式是使用Angular的依赖注入特性访问服务。
AngularJS同其他Javascript框架的一个重要区别是，控制器并不是合适的位置来处理DOM操作，进行数据格式化操作或是超出模型数据外，维持状态的操作。它仅仅作为视图层和$scope模型之间的一种联系。AngularJS同样使得设置任何类型在$scope上成为可能，包括对象，并展示对象属性在视图层。
例如，我们可以在控制器MyController中，创建一个只包含name属性的person对象：<pre><code>app.controller('MyController', function($scope) { $scope.person = {    name: "Ari Lerner"  };});
</code></pre>

在ng-controller='MyController'所在的div中的任何子元素中，我们都能够得到person对象，因为它处于$scope中。例如，现在我能能够在视图层得到person对象的引用或获取person.name。
<pre>	<div ng-app="myApp">		<div ng-controller="MyController">			<h1>{{ person }}</h1>			and their name:			<h2>{{ person.name }}</h2>  		</div>	</div></pre>
我们可以看出$scope对象怎样从模型层到视图层传递信息。
同样我们可以监听事件，同应用的其他部分交互，创建应用特点的逻辑。
Angular 使用作用域来隔离视图层功能，控制器和指令（本书后面将介绍这个概念），这也是的每块功能的测试编写简单。
###控制器层次（作用域嵌套）
AngularJS应用中每个部分都有一个父作用域（在ng-app层级上，这个作用域叫$rootScope）,不管其中渲染的内容。
这里有一个例外：指令中创建的作用域是独立的作用域。 
除了独立的作用域，其他创建的作用域都遵循原型继承，即他们能够访问到父级作用域。
如果我们熟悉面向对象编程，这个行为看起来是相似的。
默认情况下，AngularJS如果从本地作用域下查找不到摸个属性，会向上在其父作用域中查找该属性或方法，如果还找不到会继续向上查询直到查找到$rootScope作用域下。如果在$rootScope下仍没有找到，他将不会更新视图层，然后继续下去。为了了解上面的过程，我们创建一个ParentController，在它里面定义了一个person对象，并包含了引用改对象的控制器ChildController：<pre><code>app.controller('ParentController', function($scope) { 
	$scope.person = {greeted: false};});app.controller('ChildController', function($scope) { 
	$scope.sayHello = function() {    	$scope.person.name = "Ari Lerner";		$scope.person.greeted = true; 
	}});
</code></pre>
如果我们视图层中ParentController中定义了ChildController，那么ChildController中$scope的父对象就是ParentController的$scope对象。
通过原型继承行为，我们可以在子作用域中引用到ParentController的$scope（父作用域）中的数据。
例如我们可以在ChildController引用到定义在ParentController中的person对象。
<pre><code><div ng-controller="ParentController"> 
	<div ng-controller="ChildController">		<a ng-click="sayHello()">Say hello</a> 
	</div>	{{ person }}</div>
</code></pre>

这种控制器的层次结构非常类似DOM本身的层次结构。
我们可以看到，当按下button按钮时，我们在ChildController里获取到了ParentController作用域的$scope.person，就像person在ChildController的$scope中定义的一样。一个好的编程习惯是尽量保持我们的控制器简洁，把而DOM交互，数据操作逻辑放在控制器中就不是一个好的习惯。
例如，下面的控制器代码过于繁重，它包含了DOM处理和很多视图层的处理逻辑：
<pre><code>
angular.module('MyController', function($scope) { $scope.shouldShowLogin = true; $scope.showLogin = function() {    $scope.shouldShowLogin = !$scope.shouldShowLogin;   }$scope.clickButton = function() { $("#btn span").html("Clicked");}$scope.onLogin = function(user) {    $http({      method: 'POST',      url: '/login',      data: {user: user }}).success(function(data) { // user}) }})
</code></pre>一个良好设计的应用会使用指令（directive）和服务（service）来处理逻辑。我们将前面的控制器，用指令和服务来重新改写，从而得到一个更简洁，更易于维护的控制器：
<pre><code>
angular.module('MyController', function($scope, UserSrv) { 
	// The content can be controlled by	// directives	$scope.onLogin = function(user) {    	UserSrv.runLogin(user);  	}})
</code></pre>