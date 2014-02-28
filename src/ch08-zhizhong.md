# Filters    过滤器

> In AngularJS, a filter provides a way to format the data we display to the user.

在Angular中，过滤器提供了一种方法来格式化我们想要展示给用户的数据。

> Angular gives us several built-in filters as well as an easy way to create our own.

Angular为我们提供了几个内置的过滤器，以及一个简单的方法来创建我们自己的过滤器。

> We invoke filters in our HTML with the | (pipe) character inside the template binding characters {{ }}.

我们可以在HTML模板的binding characters中使用 | (管道符号)来调用过滤器。

> For instance, let’s say we want to capitalize our string.

举个栗子，假设我们想将字符串变成大写字母。

> We can either change all the characters in a string to be capitalized, or we can use a filter.

我们可以将字符串中的所有字符大写，或者我们可以使用一个过滤器。

```
{{ name | uppercase }}
```

> We can also use filters from within JavaScript by using the $filter service. For instance, to use the
  lowercase JavaScript filter:

我们还可以在Javascript中利用$filter服务来使用过滤器，比如，使用 **lowercase** 过滤器：

```javascript
app.controller('DemoController', ['$scope', '$filter',
function($scope, $filter) {
$scope.name = $filter('lowercase')('Ari');
}]);
```

> To pass an argument to a filter in the HTML form, we pass it with a colon after the filter name (for multiple arguments, we can simply append a colon after each argument).

我们通过在过滤器名称后面使用一个冒号，将参数传递给HTML表单中的过滤器(对于多个参数，我们可以简单的在每个参数后面添加一个冒号)。

> For example, the number filter allows us to limit the number of decimal places a number can show.

举个栗子， **number** 过滤器允许我们限制能显示的小数位数。

> To pass the argument 2, we’ll append :2 to the number filter:

要将2作为参数传递，我们将在 **number** 过滤器后面添加 **:2** :

```
<!-- Displays: 123.46 -->
{{ 123.456789 | number:2 }}
```

> We can use multiple filters at the same time by using two or more pipes.

我们可以通过2个或更多的 **管道符号** 同时使用多个过滤器。

> We’ll see such an example in a minute when we build a custom filter.

当稍后自定义一个过滤器的时候，我们将看到这样一个栗子。

>  Before we get to that, however, let’s look at the built-in filters that come out of the box with AngularJS.

在这之前，先让我们来看看AngularJS内置的过滤器。

## Currency 货币过滤器

> The currency filter formats a number as currency.

**currency** 过滤器将数字变成货币。

>  In other words, 123 as currency looks like: {{ 123 | currency }}.

换句话说，123要显示成货币看起来是这样：```{{ 123 | currency  }}``` 。

> Currency gives us the option of displaying a currency symbol or identifier.

**Currency**过滤器给了我们显示货币标记或标识的选择。

> The default currency option is that of the current locale; however, we can pass in a currency to display.

默认是显示当前语言环境的货币选项；然而，我们可以通过一个货币显示。？

## Date 日期过滤器

> The date filter allows us to format a date based upon a requested format style.

**date** 过滤器允许我们基于一个指定格式来格式化一个日期。

> The date formatter provides us several built-in options.

**date** 过滤器给我们提供了几个内置的选项。

> If no date format is passed, then it defaults to showing mediumDate (as you can see below).

如果没有匹配的日期格式，会默认显示为 **mediumDate** (如下文所见).

> Here are the built-in localizable formats:

下面是内置的本地化格式：

```
{{ today | date:'medium' }}
{{ today | date:'short' }}
{{ today | date:'fullDate' }}
{{ today | date:'longDate' }}
{{ today | date:'mediumDate' }} <!-- Aug 09, 2013 --> {{ today | date:'shortDate' }} <!-- 8/9/13 -->
<!-- Aug 09, 2013 12:09:02 PM -->
<!-- 8/9/13 12:09 PM -->
<!-- Thursday, August 09, 2013 -->
<!-- August 09, 2013 -->
{{ today | date:'mediumTime' }} <!-- 12:09:02 PM --> {{ today | date:'shortTime' }} <!-- 12:09 PM -->
```

>





