#指令说明
本章节旨在明确当我们构建成熟的客户端应用时，指令所必要提供的所有选项和功能。

###指令定义
我们可以简单地认为指令就是我们在一个指定的节点上运行的一个简单的函数，这个函数可以在此节点上为我们提供额外的功能。

举个例子，ng-click指令给一个节点赋予这样的功能：监听click事件，并在其接收到该事件时，执行一个Angular表达式。指令使得Angular框架非常强大，并且，如我们所见，我们还可以自己创建它。

我们可以通过 `.directive()`方法定义一个指令。当然，`.diretive()`方法只是我们应用中的Angular模块上的许多可用的方法之一。

```
angular.module('myApp') .directive('myDirective', function($timeout, UserDefinedService) {  // 在此处定义指令})
```
`.directive()`方法有2个参数：

**name(string)**

指令名称，字符串。我们会在视图中引用它。

**factory_function (function)**

factory函数需要返回一个对象，该对象定义了指令的行为。这个对象提供一些配置项，用于告知 `$compile`服务，该指令被应用于DOM中时，该如何行事。

```
angular.application('myApp', [])
.directive('myDirective', 	function() {		// 返回一个用于定义指令的对象		return {		// 在此处重写配置项来定义指令		}; 
});
```
我们也可以返回一个函数来定义指令，但最佳实践还是返回一个对象。如果返回的是函数，它通常会作为`postLink`函数被引用，我们可以用`postLink`为指令定义`link`。通过返回函数来定义指令，会使我们自定义指令时的灵活性受限，因此，这种做法仅适合一些简单地指令实现。

当Angular启动我们的应用时，会注册这个返回的对象，并且以传入的第一个参数将其命名。Angular编译器在查找指令时，会分析我们的主HTML文档，查找DOM中使用了这个名称的元素、属性（attribute）、注释或者class名称。当它找到匹配的元素时，将会使用指令定义把DOM元素放置在页面中。

```
<div my-directive></div>
```

