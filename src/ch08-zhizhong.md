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

我们可以将字符串中的所有字符变成大写，或者我们可以使用一个过滤器。

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

我们可以通过2个或更多的 **管道符号** 来同时使用多个过滤器。

> We’ll see such an example in a minute when we build a custom filter.

当稍后自定义一个过滤器的时候，我们将看到这样一个栗子。

>  Before we get to that, however, let’s look at the built-in filters that come out of the box with AngularJS.

在这之前，先让我们来看看AngularJS内置的过滤器。

### Currency 货币过滤器

> The currency filter formats a number as currency.

**currency** 过滤器将数字变成货币。

>  In other words, 123 as currency looks like: {{ 123 | currency }}.

换句话说，123要显示成货币看起来是这样：```{{ 123 | currency  }}``` 。

> Currency gives us the option of displaying a currency symbol or identifier.

**Currency**过滤器给我们提供了货币显示符号或标识的可选参数。

> The default currency option is that of the current locale; however, we can pass in a currency to display.

默认的参数配置是显示当前语言环境的货币符号；然而，我们可以通过传入一个货币符号来改变显示。

### Date 日期过滤器

> The date filter allows us to format a date based upon a requested format style.

**date** 过滤器允许我们基于一个指定格式来格式化一个日期。

> The date formatter provides us several built-in options.

**date** 过滤器给我们提供了几个内置的选项。

> If no date format is passed, then it defaults to showing mediumDate (as you can see below).

如果没有传入日期格式的参数，会默认显示为 **mediumDate** (如下文所见).

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

**date** 格式化程序还能允许我们定制自己喜欢的日期格式。                 ？

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

### filter 条件过滤器

> The filter filter selects a subset of items from an array of items and returns a new array.

条件过滤器从一个数组中筛选出一个子集，并将该子集以一个新的数组的形式返回。

> This filter is generally used as a way to filter out items for display. For instance,

该过滤器通常用于作为一种筛选出要显示条目的方法。比如，

> when using client-side searching, we can filter out items from an array immediately.

在客户端进行本地搜索的时候，我们可以从一个数组中立刻筛选出匹配的结果。

> The filter method takes a string, object, or function that it will run to select or reject array elements.

过滤器方法接受一个 **string**, **object** ,或者一个运行选择或剔除数组元素的 **function** 作为参数。

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

过滤器只会比较传入对象的属性名，这就像只传入一个字符串进行部分字符比较。

> If we want to match against all properties, we can use the $ as the key.

如果我们想要匹配对象中所有的属性，我们可以使用 **$** 作为关键字。

> function

### function 方法

> It will run the function over each element of the array, and the results that return as non-falsy will appear in the new array.

它会对数组中的每一个元素运行该方法，并将方法中返回值为真的元素添加进一个新的数组并返回该数组。

> For instance, selecting all of the words that have the letter e in them, we could run our filter like so:

举个例子，在下面这些单词中，选出所有包含字母e的单词，我们可以这样使用过滤器：

```
{{ ['Ari', 'Lerner', 'Likes', 'To', 'Eat', 'Pizza'] | filter:'e' }}
<!-- ["Lerner","Likes","Eat"] -->
```

> If we want to filter on objects, we can use the the object filter notation as we discussed above.

如果我们想要对对象进行过滤， 可以使用上面所讨论的对象过滤器。

> For instance, if we have an array of people objects with a list of their favorite foods, we could filter them like so:

举个例子，如果我们有一个数组，其中是人们喜欢的食物，我们可以像这样过滤它们：

```

{{ [{
    'name': 'Ari',
    'City': 'San Francisco',
    'favorite food': 'Pizza'
    }, {
    'name': 'Nate',
    'City': 'San Francisco',
    'favorite food': 'indian food'
    }] | filter:{'favorite food': 'Pizza'} }}
<!-- [{"name":"Ari","City":"San Francisco","favorite food":"Pizza"}] -->
```

> We can also filter based on a function that we define (in this example, on the containing $scope object):

我们也可以我们定义的函数来过滤（在这个例子中，包含 **$scope** 作用域对象）：

```
{{ ['Ari', 'likes', 'to', 'travel'] | filter:isCapitalized }}
<!-- ["Ari"] -->

```

> The isCapitalized function, which returns true if the first character is a capital letter and false if it is not, is defined as:

**isCapitalized** 函数的意思是，如果第一个字母大写返回true，否则返回false,定义如下：

```
$scope.isCapitalized =
function(str) { return str[0] == str[0].toUpperCase(); }
```

> We can also pass a second parameter into the filter method that will be used to determine if the expected value and the actual value should be considered a match.

```
We can also pass a second parameter into the filter method that will be used to determine if the expected value and the actual value should be considered a match.
If the second parameter passed in is:
true
It runs a strict comparison of the two using angular.equals(expected, actual). false
It looks for a case-insensitive substring match.
function
It runs the function and accepts an element if the result of the function is truthy.
``` ?

### JSON JSON过滤器

> The json filter will take a JSON, or JavaScript object, and turn it into a string.

**json** 过滤器会将JSON数据或者JavaScript对象，转换成字符串。

```
{{ {'name': 'Ari', 'City': 'San Francisco'} | json }}
<!-- {
  "name": "Ari",
  "City": "San Francisco"
}
-->
```

> The limitTo filter creates a new array or string that contains only the specified number of elements, either taken from the beginning or end, depending on whether the value is positive or negative.

limitTo过滤器创建一个新的数组或字符串,仅包含从开始或结束位置指定数量的元素,这取决于该值是正的还是负的。

> If the limit exceeds the value of the string, then the entire array or string will be returned.

** 如果超过了字符串或数组的长度，整个字符串或数组将都会被返回。 **

> For instance, we can take the first three letters of a string:

举个例子，我们想要字符串中的前三个字符：

```
{{ San Francisco is very cloudy | limitTo:3 }}
<!-- San -->
```

> Or we can take the last 6 characters of a string:

或者我们想要字符串中最后6个字符：

```
{{ San Francisco is very cloudy | limitTo:-6 }}
<!-- cloudy -->
```

> We can do the same with an array. Here we’ll return only the first element of the array:

我们可以对一个数组做同样的操作。这里将仅返回数组中的第一个元素：

```
{{ ['a', 'b', 'c', 'd', 'e', 'f'] | limitTo:1 }}
<!-- ["a"] -->
```

### lowercase 小写过滤器

> The lowercase filter simply lowercases the entire string.

小写过滤器只是将整个字符串变成小写。

```
{{ "San Francisco is very cloudy" | lowercase }}
<!-- san francisco is very cloudy -->
```

### number  数字过滤器

> The number filter formats a number as text.

**number** 过滤器将数字格式化成文本。

> It can take a second parameter (optional) that will format the number to the specified number of decimal places (rounded).

它可以通过第二个参数（可选），格式化数字的小数位数（四舍五入）。

> If a non-numeric character is given, it will return an empty string.

** 如果传入的是一个非数字的字符，它将返回一个空的字符串。 **

```
{{ 123456789 | number }}
<!-- 1,234,567,890 -->
{{ 1.234567 | number:2 }}
<!-- 1.23 -->
```

### orderBy 排序过滤器

> The orderBy filter orders the specific array using an expression.

**orderBy** 过滤器使用一个表达式筛选出一个特定的数组。

> The orderBy function can take two parameters: The first one is required, while the second is optional.

**oderBy** 过滤器方法可以接受两个参数：第一个参数是必须的，第二个参数是可选的。

> The first parameter is the predicate used to determine the order of the sorted array.

第一个参数是用于确定排序后的数组中顺序的谓词。

> If the first parameter passed in is a(n):

如果第一个参数是一个:

> function

**function** 函数

> It will use the function as the getter function for the object.?

**string** 字符串

> It will parse the string and use the result as the key by which to order the elements of the array.

它将解析字符串，并用解析后的结果作为关键字对数组中的元素进行排序。

> We can pass either a + or a - to force the sort in ascending or descending order.

我们可以使用一个 **+** 或者 一个 **-** 强制按升序或降序排序。

**array** 数组

> It will use the elements as predicates in the sort expression.

它将在排序表达式中使用元素作为谓词。

> It will use the first predicate for every element that is not strictly equal to the expression result.     ？

> The second parameter controls the sort order of the array (either reversed or not).

第二个参数控制数组的排序顺序（反向与否）。

> For instance, let’s sort an array of objects by their name.

举个例子，让我们以name来对一个对象数组进行排序。

> Say we have an array of people, we can order the array of objects with the name value:

假设我们有一个包含 **people** 对象的数组，我们可以使用name的值来对数组进行排序：

```
{{ [{
    'name': 'Ari',
    'status': 'awake'
    }, {
    'name': 'Q',
    'status': 'sleeping'
    }, {
    'name': 'Nate',
    'status': 'awake'
    }] | orderBy: 'name' }}
<!-- [
  {"name":"Ari","status":"awake"},
  {"name":"Nate","status":"awake"},
  {"name":"Q","status":"sleeping"}
  ]
-->
```

> We can also reverse-sort the object.

我们也可以反向排序对象。

> For instance, reverse-sorting the previous object, we simply add the second parameter as true:

举个例子，反向排序前面的对象，我们只需要简单的设置第二个参数为true:

```
{{ [{
    'name': 'Ari',
    'status': 'awake'
    }, {
    'name': 'Q',
    'status': 'sleeping'
    }, {
    'name': 'Nate',
    'status': 'awake'
    }] | orderBy:'name':true }}
<!-- [
  {"name":"Q","status":"sleeping"},
  {"name":"Nate","status":"awake"},
  {"name":"Ari","status":"awake"}
  ]
-->
```

### uppercase 大写过滤器

> The uppercase filter simply uppercases the entire string:

**uppercase** 过滤器简单的将字符串全部变成大写：

```
{{ "San Francisco is very cloudy" | uppercase }}
<!-- SAN FRANCISCO IS VERY CLOUDY -->
```

> Making Our Own Filter

## 创建自己的过滤器

> As we saw above, it’s really easy to create our own custom filter.

如我们前面所见，创建属于我们自己的过滤器是很容易的。

> To create a filter, we put it under its own module.

要创建过滤器，我们要将它放在module中。

> Let’s create one together: a filter that capitalizes the first character of a string.

让我们一起来创建一个将字符串的第一个字母大写的过滤器。

> First, we need to create it in a module that we’ll require in our app (this step is good practice):

首先，我们需要在module中创建它，在我们的app中我们会需要module(这一步是最佳实践):？

```
angular.module('myApp.filters', []) .filter('capitalize', function() {
return function(input) {} });
```

> Filters are just functions to which we pass input.

过滤器仅仅是一个接受input参数的函数。？

> In the function above, we simply take the input as the string on which we are calling the filter.

在上面的函数中，我们只是以input作为字符串当作参数来调用过滤器。

> We can do some error checking inside the function:

我们可以在函数中做一些错误检查：

```
angular.module('myApp.filters', []) .filter('capitalize', function() {
return function(input) {
// input will be the string we pass in if (input)
return input[0].toUpperCase() + input.slice(1);
} });
```

> Now, if we want to capitalize the first letter of a sentence, we can first lowercase the entire string and then capitalize the first letter with our filter:

现在，如果我们想把句子的首字母大写，我们可以先将全部字符串小写，然后使用我们的过滤器大写第一个字母：

```
<!-- Ginger loves dog treats -->
{{ 'ginger loves dog treats' | lowercase | capitalize }}
```

## 表单验证

> When taking input from our users, it’s important to show visual feedback on their input.

当用户在填写表单的时候，对填写内容的验证反馈是很重要的。

> In the context of human relationships, form validation is just as much about giving feedback as well as getting the “right” input.

借鉴人和人之间的关系，表单验证就像是获取反馈并获得正确输入一样。

> Not only does it provide positive feedback for our user, it will also semi-protect our web app from bad or invalid input.

它不仅对用户来说是积极的反馈，同时也能保护我们的web-app免受错误或无效输入的干扰。

> We can only protect our back end as much as is possible with our web front end.

只能通过前端的努力，保护后端程序成为可能。

> Out of the box, AngularJS supports form validation with a mix of the HTML5 form validation inputs as well as with its own validation directives.

AngularJS对表单验证，支持使用HTML5的验证标签和AngularJS提供的验证指令混合使用。

> There are many form validation directives available in AngularJS. We’ll talk about a few of the core validations, then we’ll get into how to build your own validations.

在AngularJS中有很多表单验证指令。我们会讨论一部分核心的验证指令，后续还会深入研究如何创建自己的验证指令。

```
<form name="form" novalidate>
<label name="email">Your email</label>
<input type="email" name="email" ng-model="email" placeholder="Email Address" />
</form>
```

> AngularJS makes it pretty easy for us to handle client-side form validations without adding a lot of extra effort.

没做很多额外的工作，AngularJS就为我们搞定了客户端的表单验证。

> Although we can’t depend on client-side validations to keep our web application secure, they do provide instant feedback of the state of the form.

虽然我们不能依赖客户端的验证来保持web-app的安全，但它对表单输入提供了即时的反馈。

> To use form validations, we first must ensure that the form has a name associated with it, like in the above example.

使用表单验证，我们首先要确保该表单有一个名字与之关联，就像上面例子中的写法。

> All input fields can validate against some basic validations, like minimum length, maximum length, etc. These are all available on the new HTML5 attributes of a form.

所有的input都可以使用HTML5中提供的新的表单验证属性，来做一些基础的验证，比如最小长度，最大长度等等。

> It is usually a great idea to use the novalidate flag on the form element, as it prevents the browser from natively validating the form.

在form元素上使用novalidate标识，防止浏览器进行本地验证是一个好主意。

> Let’s look at all the validation options we have that we can place on an input field:

让我们来一起看一下可以使用在input上的验证选项：

#### Required    必填项

> To validate that a form input has been filled out, we simply add the HTML5 tag, required, to the input field:

为了验证表单中的input已经被填写，我们只是简单的将HTML的required标签添加到这个input上：

```
<input type="text" required />
```

#### Minimum Length  最小长度

> To validate that a form input input is at least a certain {number} of characters, we add the AngularJS directive ng-minlength="{number}" to the input field:

为了验证表单中的input需要输入的最少字符数量，我们向input中添加AngularJS的指令ng-minlength="{number}":

```
<input type="text" ng-minlength=5 />
```

#### Maximum Length  最大长度

> To validate that a form input is equal to or less than a number of characters, we can add the AngularJS directive ng-maxlength="{number}":

为了验证表单中input能输入的多大字符数量，我们向input中添加AngularJS的指令ng-maxlength="{number}":

```
<input type="text" ng-maxlength=20 />
```

#### Matches a Pattern   正则表达式

> To ensure that an input matches a regex, we can use ng-pattern="/PATTERN/":

为了确保input的输入符合正则表达式，我们可以使用ng-pattern="/PATTERN/":

```
<input type="text" ng-pattern="/a-zA-Z/" />
```

#### Email   邮箱地址

> To validate an email address in an input field, we simply set the input type to email, like so:

为了验证input中的邮箱地址，我们可以简单的将input的type属性设置成email，比如：

```
<input type="email" name="email" ng-model="user.email" />
```

#### Number  数字

> To validate an input field has a number, we set the input type to number:

为了验证input只接受输入数字，我们将input的type属性设置成number：

```
<input type="number" name="age" ng-model="user.age" />
```

#### URL

> To validate that an input represents a URL, set the input type to url:

为了验证input中的输入表示一个URL地址，将input的type属性设置成url：

```
<input type="url" name="homepage" ng-model="user.facebook_url" />
```

### Custom Validations  自定义验证选项

> AngularJS makes it very easy to add our own validations, as well, by using directives. For instance, let’s say that we want to validate that our username is available in the database.

AngularJS可以很容易的添加属于我们自己的验证选项，并通过指令的形式使用它们。举个例子，假如我们想验证下username在数据库中是否可用。

```
angular.module('validationExample', []) .directive('ensureUnique', function($http) {
return {
require: 'ngModel',
link: function(scope, ele, attrs, c) {
scope.$watch(attrs.ngModel, function() { $http({
          method: 'POST',
          url: '/api/check/' + attrs.ensureUnique,
          data: {'field': attrs.ensureUnique}
}).success(function(data,status,headers,cfg) { c.$setValidity('unique', data.isUnique);
}).error(function(data,status,headers,cfg) { c.$setValidity('unique', false);
}); });
} }
});
```



































