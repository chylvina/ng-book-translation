#Scopes（作用域）
Scope(作用域)是所有Angular应用的核心组成部分。它们的使用场景非常广泛，所以我们非常有必要去了解他们的运行机制。  
scope在angular中的角色就像是数据模型一样。scope为表达式提供执行的上下文。在我们构建业务逻辑代码、编写控制器（controller）中的方法或者定义模板（view）的属性的时候，我们都能发现scope的身影。  
Scope扮演着控制器（controller）和视图（view）之间的胶水角色。在我们的程序渲染视图（view）之前，视图的模板会链接到相应的scope上，当程序开始构建DOM树的时候会通知angular进行适当的改变。这种异步的特性和promises机制异曲同工，就像一个XHR请求的过程，发出请求，然后进行异步回调，想要完全理解，请看promises章节。  
Scope是Angular应用的核心。因为动态数据绑定的缘故，当我们在视图（view）中做出修改的时候，$scope对象会立即进行更新。同样，我们也可以在$scope对象改变的时候更新视图（view）。  
在AngularJS中，$scope的机制类似DOM树的机制，是按照层次排布的，换句话来讲，我们可以在一个子$scope对象中可以获取其父$scope对象的属性  
    如果你很熟悉javascript的上下文机制,那么这种分层的理念就不难理解。在javascript创建一个可执行上下文的时候，例如我们可以创建一个新的函数，那么这个函数将会自动创建了一个局部上下文。在Angular中$scope的概念也是这样的，每当我们为一个DOM元素的子DOM元素创建一个scope，那么我们就在当前所在的父DOM的scope之下创建了一个新的可执行的上下文。
scope提供了一种能够监听model改变的能力，对于开发者来讲，可以通过调用scope的apply方法，传播model改变的事件，我们在一个scope的上下文中定义执行的表达式，我们也可以传播事件到其他的controllers或者程序的某些区域（此事件可以传播的区域）。  
将程序的业务逻辑放置在controller中，并将需要用到的数据也放置到这个controller的scope中是一个非常好的方式。  
###The $scope View of the World
在Angular开始执行并生成视图的时候，会创建一个从root ng-app元素到$rootScope作用域的数据绑定。$rootScope是所有$scope对象的最终父元素。  
####scopes
  $rootScope对象是距离全局上下文最近的作用域对象，在全局上下文上绑定过多的业务逻辑是一个糟糕的方式，同样也不要在$rootScope上绑定过多的属性，这样会污染全局的作用于。 
    $scope对象是普通的javascript对象,我们很方便的去修改$scope中的属性。
    $scope对象是Angular中的数据模型，并不像传统像其他语言中传统的数据对象，$scope不再提供对数据处理的方法（setter,getter，或者自定义的方法），请记住$scope就是普通的javascript对象，$scope是视图（view）和HTML之间的桥梁，是视图和控制器之间的纽带。  
  所有在$scope中可以访问的属性，在对应的view中都可以获取。  
  例如我们来看一段HTML代码：
<pre>
<code>
&lt;div ng-app="myApp"> 
    &lt;h1>Hello {{ name }}&lt;/h1>&lt;/div>
</code>
</pre>
我们可以猜测到{{name}}变量是在包含此元素的$scope的一个属性
<pre>
<code>
angular.module('myApp', []) .run(function($rootScope) {    $rootScope.name = "World";});
</code>
</pre>
<center><img src="http://ringtail.u.qiniudn.com/ng-book-01"/></center>
####It's Just HTML
在应用中会将HTML进行渲染，然后将渲染好的HTML在浏览器中显示。HTML中包含所有的标准的HTML元素，既包含有Angular独特的元素，也包含非Angular的元素。不包含Angular独特属性的元素无需特别的声明。  
#####Scopes
<pre>
<code>
&lt;h2>Hello world&lt;/h2>
&lt;h3>Hello {{ name }}&lt;/h3>
</code>
</pre>
在上面的例子中，当scope变化的时候，会自动更新&lt;h3>,Angular无需处理&lt;h2>元素。  
在Angular中，我们可以在模板中使用多种标识，这些标识包括以下几种：
    * Directives：
    * 数值绑定：在模板中使用 {{ }} 将表达式绑定到视图上（view）
    * 过滤器(Filters):在视图中（view）起格式化作用的函数
    * 表单控制（Form controls):表单验证控制  
###scope可以做什么
Scope有以下的基本作用：  
* 时时监听model的变化  
* 提供了在整个应用中以及和其他组件之间传递model变化的方法  
* scope由于是分层模型，所以可以隔离功能代码和模型的属性  
* 提供了表达式运行的执行上下文  
在我们开发Angular app的时候，最主要的工作就是构建在不同scope下的功能代码
scope是包含功能代码和渲染模板的数据的对象，它是所有的views的核心，你可以把scope想象成view(视图)的数据模型。  
在之前的例子中，我们在$rootScope中设置了一个变量，然后在view中进行了引用，就像下下面的代码：  
<pre>
<code>
angular.module('myApp', []) .run(function($rootScope) {    $rootScope.name = "World";});
</code>
</pre>
现在我们可以在view中使用name这个属性了：
<pre>
<code>
&lt;div ng-app="myApp"> 
    &lt;h1>Hello {{ name }}&lt;/h1>&lt;/div>
</code>
</pre>  
如果我们不在$rootScope中设置变量，我们可以创建一个controller的方式建立子scope.我们可以通过在一个DOM元素上使用ng-controller directive的方式建立一个controller:  
<pre>
<code>
&lt;div ng-app="myApp">    &lt;div ng-controller="MyController">        &lt;h1>Hello {{ name }}&lt;/h1>
    &lt;/div>&lt;/div>
</code>
</pre>
现在，我们已经创建了一个叫做MyController的controller，从而可以管理变量：
<pre>
<code>
angular.module("myApp", []) .controller('MyController', function($scope) {  $scope.name = "Ari";});
</code>
</pre>
ng-controllers dirctive为当前的元素创建了一个新的$scope，而这个作用域在$rootScope之下。  
###$scope的生命周期
在Angular运行上下文中，当浏览器接收到一个Javascript的回调函数的时候，$scope将会识别出model的变化（想获得更多关于Angular运行上下文的信息，可以查看digest loop章节）  
<img style="display:block;float:left;"src="http://ringtail.u.qiniudn.com/ng-book-00"/>
<p style="padding-top:5px">如果回调函数在Angular的上下文之外运行，我们也可以采用$apply方法进行监听变化<p>  
在scope表达式执行完毕，$disget循环执行之后，$scope的watch方法就会进行脏数据的校检（关于脏数据校检，请查看disget loop章节）  
<img style="display:block;float:left;"src="http://ringtail.u.qiniudn.com/ng-book-00"/>
<p style="padding-top:5px">我们将会在Expressions章节中深入学习expressions。在scope中的表示式就是我们设置scope变量的位置。当我们想像上面的代码中设置name属性，我们只需要使用：$scope.name="Ari"<p>    
###Creation
当我们创建一个controller或者directive，angular会在运行时调用$injector创建一个新的作用域，并将这个作用域传递给controller或者dirctive
###Linking
当$scope被链接到视图的时候，所有创建scope的directives会将其监听器注册到父scope上。这些监听器会监听从视图层到指令的数据模型变化并进行传播。
###Update
当$digest循环在$rootScope中执行的时候，所有的子scope都会进行脏数据的校检。这时会检查所有监听表达式是否有变化，一旦发现任何的变化，scope都会调用监听者的回调函数。
###Destruction
当一个$scope不在需要的时候，该scope的创建者会调用scope.$destroy()方法清理该scope。
注意当一个scope被销毁了，$destory事件会被广播。
###directives and Scopes
Directives会在angluar程序中广泛使用，通常情况下无需创建属于他们自己的作用域，但是这取决于这个directive的用途。例如：ng-controller 和 ng-repeat direcitve 会创建属于他们的子scope，并且将scope绑定到dom元素上。  
但是在我们学习更多时候，我们需要了解什么是controllers,我们如何在我们的程序中使用。
