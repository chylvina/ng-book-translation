#指令说明
本章节旨在明确当我们构建成熟的客户端应用时，指令所必要提供的所有选项和功能。

###指令定义
我们可以简单地认为指令就是我们在一个指定的节点上运行的一个简单的函数，这个函数可以在此节点上为我们提供额外的功能。

举个例子，ng-click指令给一个节点赋予这样的功能：监听click事件，并在其接收到该事件时，执行一个Angular表达式。指令使得Angular框架非常强大，并且，如我们所见，我们还可以自己创建它。

我们可以通过 `.directive()`方法定义一个指令。当然，`.diretive()`方法只是我们应用中的Angular模块上的许多可用的方法之一。

```
angular.module('myApp') .directive('myDirective', function($timeout, UserDefinedService) {

`.directive()`方法有2个参数：

**name(string)**

指令名称，字符串。我们会在视图中引用它。

**factory_function (function)**

factory函数需要返回一个对象，该对象定义了指令的行为。这个对象提供一些配置项，用于告知 `$compile`服务，该指令被应用于DOM中时，该如何行事。

```
angular.application('myApp', [])
.directive('myDirective', 	function() {
});



当Angular启动我们的应用时，会注册这个返回的对象，并且以传入的第一个参数将其命名。Angular编译器在查找指令时，会分析我们的主HTML文档，查找DOM中使用了这个名称的元素、属性（attribute）、注释或者class名称。当它找到匹配的元素时，将会使用指令定义把DOM元素放置在页面中。

```
<div my-directive></div>
```

> 为了避免和未来的HTML标准冲突，最好的做法是通过给自定义的指令一个前缀来定义其命名空间。Angular本身已经选择了`ng-`前缀，所以请使用其它前缀。在下面的例子中，我们将使用`my-`前缀(比如`my-directive`)。

我们为一个指令定义的factory函数只会在编译器第一次匹配到指令时被调用*一次*。和 `.controller`函数一样，我们使用`$injector.invoke`调用指令的factory函数。


.directive('myDirective', function() {
	compile: return an Object OR
			return {



下面是可选的选项：

* E（元素）`<my-directive></my-directive>`
* A（arribute，默认） `<div my-directive="expression"></div>`
* C（class）`<div class="my-directive: expression;"></div>`
* M（注释） `<– directive: my-directive expression –>`

这些选项可以单独使用，也可以合并使用：

```
angular.module('myDirective', function() { 
	return {
	};
在这个例子中，我们既可以通过属性（attribute）也可以通过元素名称来声明指令：

```
<-- as an attribute -->
<my-directive></my-directive>


> 尽量避免使用注释来声明一个指令，这种格式最早被作为定义包含多个DOM元素的指令的一种方法。当`ng-repeat`包含一个`<table>`元素时，这种方法显得非常有用；然而，从Angular 1.2开始，`ng-repeat`提供了`ng-repeat-start`和`ng-repeat-end`作为这种问题的更好的解决方案，使得以注释形式定义指令的需要降到最低。当然，如果你很好奇，可以在使用ng-repeat时打开Chrome开发者工具的`元素`标签，看看注释的使用。


**元素还是属性（Attribute）？**

当我们需要在页面上创建一些新的东西，那么使用元素定义指令将会封装成一个独立功能区块。举个例子，如果我们要创建一个“钟”（不考虑低版本的IE），我们创建一个“钟”的指令并且在DOM中这样声明：

```
<my-clock></my-clock>
```
这样做是在告诉我们指令的用户，我们在应用程序中指定了一个完整的功能区块。我们的“钟”并不是在装饰或者扩展一个已存在的“钟”，相反，它声明了一个完整的单元。在这种场景下，我们也可以使用属性（Angular并不关心），但我们选择了使用元素，因为这样明确了我们的意图。

当我需要使用数据或者行为装饰一个已存在的元素时，使用属性。沿用上面的“钟”的例子，假设我们对一个模拟版的“钟”感兴趣：

```
<my-clock clock-display="analog"></my-clock>
```

元素还是属性的选择通常取决于，指令的定义是提供了一个页面组件的核心行为，还是通过一些可选的行为、状态或者其它在编程中可能遇到的东西（比如时钟的模拟输出）来装饰或扩展一个核心指令。

这里的指导性原则是，指令的格式应该向应用程序表达每一个功能区块的意图，创建易于理解和共享的示范性代码。

对于一个指令来说，另一个重要的区分是它是否从它包含的环境作用域中创建、继承或剥离。这种父子关系在指令的组成和可重用性上扮演了另一个关键的角色，我们将在讨论指令作用域时花更多的时间讨论这个话题。