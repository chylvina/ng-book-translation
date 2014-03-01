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

> The date formatter also enables us to customize your date format to our own liking.

**date** 过滤器还能依据我们自己的喜好定制你自己的日期格式。                 ？

> We can combine and chain together these format options to create one single date format, as well:

我们可以组合并将这些格式选项结合在一起，去创建一个单独的日期格式：

**Year Formatting**

```
Four-digit year: {{ today | date:'yyyy' }} <!-- 2013 --> Two-digit padded year: {{ today | date:'yy' }} <!-- 13 --> One-digit year: {{ today | date:'y' }} <!-- 2013 -->
```

**Month Formatting**
```
Month in year: {{ today | date:'MMMM' }} <!-- August --> Short month in year: {{ today | date:'MMM' }} <!-- Aug --> Padded month in year: {{ today | date:'MM' }} <!-- 08 --> Month in year: {{ today | date:'M' }} <!-- 8 -->
```

**Day Formatting**

```
Padded day in month: {{ today | date:'dd' }} <!-- 09 --> Day in month: {{ today | date:'d' }} <!-- 9 -->
Day in week: {{ today | date:'EEEE' }} <!-- Thursday --> Short day in week: {{ today | date:'EEE' }} <!-- Thu -->
```
**Hour Formatting**

```
Padded hour in day: {{ today | date:'HH' }} <!-- 00 --> Hour in day: {{ today | date:'H' }} <!-- 0 -->
Padded hour in am/pm: {{ today | date:'hh' }} <!-- 12 --> Hour in am/pm: {{ today | date:'h' }} <!-- 12 -->
```

**Minute Formatting**

```
Padded minute in hour: {{ today | date:'mm' }} <!-- 09 --> Minute in hour: {{ today | date:'m' }} <!-- 9 -->
```

**Second Formatting**

```
Padded second in minute: {{ today | date:'ss' }} <!-- 02 -->
Second in minute: {{ today | date:'s' }} <!-- 2 -->
Padded millisecond in second: {{ today | date:'.sss' }} <!-- .995 -->
```

**String Formatting**

```
am/pm character: {{ today | date:'a' }} <!-- AM -->
4-digit representation of time zone offset: {{ today | date:'Z' }} <!-- -0700 -->
```

> Some examples of custom date formatting:

一些自定义的日期格式：

```
{{ today | date:'MMM d, y' }} <!-- Aug 09, 2013 -->
{{ today | date:'EEEE, d, M' }} <!-- Thursday, 9, 8 --> {{ today | date:'hh:mm:ss.sss' }} <!-- 12:09:02.995 -->
```

## filter 条件过滤器

> The filter filter selects a subset of items from an array of items and returns a new array.

条件过滤器从一个数组中筛选出一个子集，并将该子集以一个新的数组的形式返回。

> This filter is generally used as a way to filter out items for display. For instance,

该过滤器通常用于作为一种方法来筛选出要显示的条目。比如，

> when using client-side searching, we can filter out items from an array immediately.

使用客户端搜索的时候，我们可以从一个数组中立刻筛选出匹配的结果。

> The filter method takes a string, object, or function that it will run to select or reject array elements.

过滤器方法接受一个 **string**, **object** ,或者一个将运行选择或剔除数组元素的 **function** 作为参数。

> If the first parameter passed in is a:

如果传入的第一个参数是：

> string

### string 字符串

> It will accept all elements that match against the string.

过滤器会返回所有匹配该string的元素。

> If we want all the elements that do not match the string, we can prepend a ! to the string.

如果我们想要所有不匹配该string的元素，我们可以在这个string前加一个 **!** 。

> object

### object 对象

> It will compare objects that have a property name that matches,as with the simple substring match if only a string is passed in.

如果只有一个字符串传入，过滤器会将对象的属性名与传入的字符串进行比较。 ？

> If we want to match against all properties, we can use the $ as the key.

如果我们想要匹配对象中所有的属性，我们可以使用 **$** 作为关键字。

> function

### function 方法













