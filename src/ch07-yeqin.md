##表达式(p31-p36)

表达式在AngularJS中非常常见，所以理解什么是表达式，AngularJS怎样使用表达式，怎样计算表达式的值对我们来说很重要。
在前面的例子中我们已经接触过Angular表达式，以{{}}标记，联系在$scope中的变量实际上就是一个表达式：{{ expression }}
当设置了$watch，我们使用表达式（或函数），Angular会计算他们的值。
表达式与javascript中eval结果非常相似。
Angular处理他们，因此，他们有如下重要，独特的性质：

- 所有表达式在作用域上下文中执行，并能够获取本地$scope变量

- 表达式可以接受一个过滤器，并可以进行过滤器的链式操作
表达式在使用他们的作用域上进行操作。这使得我们可以使用表达式中绑定在作用域上的变量，同样我们能够在作用域中循环变量（稍后可以看到ng-repeat的循环过程），调用函数，或使用数学表达式的结果。

尽管Angular应用通过$digest循环为你自动解析表达式，有时也我们需要自己解析表达式。


<pre><code>
<div ng-controller="MyController"> 
	<input ng-model="expr" type="text" placeholder="Enter an expression" /> 	<h2>{{ parsedValue }}</h2>
</code></pre>
<pre>
angular.module("myApp", []) .controller('MyController', function($scope, $parse) {
*See it live here18.*

<pre>
angular.module('myApp', [])
</pre>
The $interpolate service takes up to three parameters, with only one required function.



<div ng-controller="MyController"> <input ng-model="to"


angular.module('myApp', [])




Now that we have created this module, we can inject it into our app and run the email parser on the text in our email body:
<pre>
angular.module('myApp', ['emailParser'])
</pre>

<div id="emailEditor"> <input ng-model="to"
