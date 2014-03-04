##表达式(p31-p36)

表达式在AngularJS中非常常见，所以理解什么是表达式，AngularJS怎样使用表达式，怎样计算表达式的值对我们来说很重要。
在前面的例子中我们已经接触过Angular表达式，以{{}}标记，联系在$scope中的变量实际上就是一个表达式：{{ expression }}
当设置了$watch，我们使用表达式（或函数），Angular会计算他们的值。
表达式与javascript中eval结果非常相似。
Angular处理他们，因此，他们有如下重要，独特的性质：

- 所有表达式在作用域上下文中执行，并能够获取本地$scope变量
- 如果表达式导致类型错误或引用错误，它并不抛出错误异常。- 他们不包含任何控制流（比如：条件控制，if/else语句）
- 表达式可以接受一个过滤器，并可以进行过滤器的链式操作
表达式在使用他们的作用域上进行操作。这使得我们可以使用表达式中绑定在作用域上的变量，同样我们能够在作用域中循环变量（稍后可以看到ng-repeat的循环过程），调用函数，或使用数学表达式的结果。
###解析Angular表达式
尽管Angular应用通过$digest循环为你自动解析表达式，有时也我们需要自己解析表达式。Angular evaluates expressions by an internal service (called the $parse service) that has knowledge of the current scope. This setup gives us access to the raw JavaScript data and functions that are defined on our $scope.
To manually parse an expression, we can inject the $parse service into a controller and call the service to do the parsing for us. For instance, if we have an input box in our page that’s bound to the expr variable, like so:

<pre><code>
<div ng-controller="MyController"> 
	<input ng-model="expr" type="text" placeholder="Enter an expression" /> 	<h2>{{ parsedValue }}</h2></div>
</code></pre>In MyController, we can then set a $watch and parse the expression expr.
<pre><code>
angular.module("myApp", []) .controller('MyController', function($scope, $parse) {$scope.$watch('expr', function(newVal, oldVal, scope) { if (newVal !== oldVal) {      // Let's set up our parseFun with the expressionvar parseFun = $parse(newVal);// Get the value of the parsed expression $scope.parsedValue = parseFun(scope);} });});</code></pre>
*See it live here18.*###插入字符串
Although it’s uncommon to need to manually interpolate a string template in Angular, we do have the ability to manually run the template compilation. Interpolation allows us to live update a string of text based upon conditions of a scope, for instance.To run an interpolation on a string template, we need to inject the $interpolate service in our object. In this example, we’ll inject it into a controller:
<pre>
angular.module('myApp', [])  .controller('MyController',function($scope, $interpolate) {// We have access to both the $scope // and the $interpolate services});
</pre>
The $interpolate service takes up to three parameters, with only one required function.• text (string) - The text with markup to interpolate.• mustHaveExpression (boolean) - If we set parameter to true, then the text will return null ifthere is no expression.• trustedContext (string) - Angular sends the result of the interpolation context through the$sce.getTrusted() method, which provides strict contextual escaping.
*See $sce for more details about the last parameter.*
The $interpolate service returns an interpolation function that takes a context object against which the expressions are evaluated.With these parameters set up, we can now run an interpolation inside the controller. For instance, let’s say we want to show live editing of the body of text in an email. We can run an interpolation when the text changes to show the given output.
<pre>
<div ng-controller="MyController"> <input ng-model="to"        type="email"placeholder="Recipient" /> <textarea ng-model="emailBody"></textarea> <pre>{{ previewText }}</pre></div></pre>
In our controller, we set up a $watch to monitor changes on the email body and interpolate the emailBody into our previewText property.
<pre>
angular.module('myApp', [])  .controller('MyController',function($scope, $interpolate) {// Set up a watch $scope.$watch('emailBody', function(body) {if (body) {var template = $interpolate(body); $scope.previewText =            template({to: $scope.to});        }}); });</pre>
*See this in action here19.*
Now, inside of our {{ previewText }} body, we can use {{ to }} as a variable in the text and allow it to be live-updated along with the rest of the text.
*If it’s desirable to use different beginning and ending symbols in our text, we can modify them by configuring the $interpolateProvider.*
To modify the beginning string, we can set the starting symbol with the startSymbol() method. The startSymbol() takes a single argument:• value (string) - the value to set the starting symbolTo modify the ending symbol, we can use the endSymbol() function. This function takes a single argument, as well:• value (string) - the value to set the end symbolTo modify the starting symbol, we can create a new module and inject the $interpolateProviderinto the config() function.We’ll also create a service, which we will cover in depth in the services chapter.<pre>angular.module('emailParser', [])  .config(['$interpolateProvider',function($interpolateProvider) { $interpolateProvider.startSymbol('__'); $interpolateProvider.endSymbol('__');}]).factory('EmailParser', ['$interpolate',function($interpolate) {// a service to handle parsing return {parse: function(text, context) { var template = $interpolate(text); return template(context);} }}]);</pre>
Now that we have created this module, we can inject it into our app and run the email parser on the text in our email body:
<pre>
angular.module('myApp', ['emailParser'])  .controller('MyController',['$scope', 'EmailParser', function($scope, EmailParser) {        // Set up a watch$scope.$watch('emailBody', function(body) { if (body) {            $scope.previewText =              EmailParser.parse(body, {                to: $scope.to              });} });}]);
</pre>Now, instead of requiring the text to use the default syntax with the {{ }} symbols, we can defineour symbols to use __ instead.As we’re setting the symbols to __ on either side, we’ll need to change the HTML to use this syntax instead of {{ }}:
<pre>
<div id="emailEditor"> <input ng-model="to"        type="email"placeholder="Recipient" /> <textarea ng-model="emailBody"></textarea></div><div id="emailPreview"><pre>__ previewText __</pre> </div></pre>
*See it in action here20.*