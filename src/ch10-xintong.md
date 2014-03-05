### 内置指令
Angular提供了一套内置指令。某些指令覆盖内置的html元素，如<form>和<a>标签。然而事实上，当我们在html中使用这些标签时，无法立刻获得我们想要的功能，然而Angular提供的指令可以帮助我们实现。

例如，<form>标签封装了许多功能，像表单校验，这在标准HTML的form中并不默认存在。

其他内置指令通过他们的ng-前缀被显式声明。例如，ng-href,我们下面会详细介绍，在ng-href="someExpression"被解析并得到返回值之前，链接的指向是无效的。

最后，一些内置指令并没有对应的html标签或属性，如 ng-controller，它可以使用在任何标签上做为该标签的一个属性 ，但是更多的被用在同一个作用域范围内、拥有多个子节点的父元素上。

注意，带ng-前缀的内置指令只是Angular指令库中的一部分。因此，不要使用这个前缀命名自定义指令。


### 基本的ng属性指令
第一部分指令采用了类似html属性的命名，因为只是在html属性前增加了ng的前缀所以也很容易被记住，包括：

• ng-href
• ng-src
• ng-disabled
• ng-checked
• ng-readonly
• ng-selected
• ng-class
• ng-style


### 布尔属性
下面要介绍的Angular指令有助于令HTML的布尔属性使用起来更简单。

HTML specification31 将布尔属性定义为：一个代表真假（true/false）值的属性。当属性存在，则该属性的值被假定为true（不管它的实际值是什么）。如果不存在，则属性值为false 。

通过数据绑定进行动态数据。。。时，我们不能简单的设定属性值为true或者false,因为当属性不存在时为false？？？。因此，Angular提供了一个ng-前缀的版本???

When working with dynamic data via data bindings in Angular, we cannot simply set the value of the attribute to true or false, because by definition of the spec, the attribute is false if it is not present. Thus Angular provides an ng-prefixed version of these attributes that will evaluate the expression provided to insert or remove the corresponding boolean attribute on the decorated element.

#### ng-disabled
用ng-disabled来绑定以下表单项的disabled属性：
• <input> (text, checkbox, radio, number, url, email, submit)
• <textarea>
• <select>
• <button>

当使用普通的HTML表单项时，表单项的disabled属性代表此字段禁用。可以使用ng-disabled来控制disabled这个属性是否生效。

例如，如果用户未在文本框中输入值，禁用下方的按钮

code。。。

在下一个例子中，我们禁用文本框5秒钟，直到isDisabled属性在$timeout函数中变为true  。


#### ng-readonly
与其他布尔属性处理方式一样，HTML规范只关心表单项是否存在只读属性，而不关心它是否有值 。
为了在Angular使用一个返回true/false的表达式来控制readonly属性的存在与否，需要使用ng-readonly：


#### ng-checked
在HTML中，checked是一个布尔属性，因此checked没有值。然而，为了能在Angular中使用表达式的返回值控制checked属性存在与否，需要使用ng-checked。
在下面的例子中，我们首先用ng-init将someProperty的初始值设为true。然后将someProperty的值绑定到ng-checked，最后通知Angular是否给checkbox输出checked属性，则checkbox默认选中。

在下面的例子里，我们做了相反的处理，checkbox默认不被选中：
注意： 为了演示更加直观，这里我们也为表单项的对应label标签使用ng-model绑定了someProperty和anotherProperty的值。

#### ng-selected
使用ng-selected指令为option标签指定是否存在selected属性

