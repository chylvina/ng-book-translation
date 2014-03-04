## 指令介绍

作为Web开发者，HTML是我们所熟知的。先让我们一起花点时间来回顾一下这些最基本的Web术语。

### HTML Document
一个HTML Document 是包含结构的纯文本文档，并且可以通过CSS设置样式或使用JavaScript进行操作。

### HTML Node
一个HTML Node是一个元素或者嵌套在另一个元素中的文本块。所有的元素都是节点，但一个文本节点不是一个元素。

### HTML Element
一个包含开始标签与结束标签的元素。

### HTML Tag
一个HTML Tag负责标记一个元素的开始与结束。标签本身通过尖括号声明。
一个起始标签包含其所属元素的名称，也可以包含属性，用来修饰此元素。

### Attributes
HTML 元素可以包含属性来提供一个元素的额外信息。这些属性通常被配置在起始标签上，我们可以通过一个key-value对来配置他们，例如：key=”value”，或者仅指定一个key。

让我们来看看&lt;a&gt;超链接标签，用于创建从一个页面到另一个页面的链接：
有些标签，像超链接标签，有些可以看作参数的特殊属性。比如，一个链接标签的href属性使链接标签既具有链接行为，同时在所有浏览器默认情况下也将链接标签内的文本显示为蓝色。

<pre><code>
<a href="http://google.com">Click me to go to Google</a>
</code></pre>

&lt;a&gt;标签定义了一个到另一个页面的链接，此页面属于我们的网站或在其他网站，这取决于href属性的内容，该内容定义了链接的目标。
它与下面的HTML 按钮元素有明显的区别：

<pre><code>
<button href="http://google.com" type="submit">Click me</button>
</code></pre>

链接标签在默认情况下带下划线且为蓝色，而Button默认情况下在我们的浏览器里看起来是一个可点击的按钮。
链接标签知道，当指定href属性为http://google.com时，如果用户点击链接，它应该改变地址栏中的URL并加载谷歌的首页。
而按钮标签，完全无视指定的href属性，也不执行跳转的行为（该属性被忽略）。
因此，改变地址栏中的URL并把你带到一个新的页面是一个链接的预置行为，但不是按钮的预置行为。
不过这两个标签对title属性提供相同的行为：当用户悬停时都显示提示。

<pre><code>
<a href="http://google.com" title="click me">Click me to go to Google</a>
<button type="submit" title="click me">Click me</button>
</code></pre>

总之，Web浏览器为我们呈现HTML 元素样式和行为的能力是Web的基本优势之一。
每个供应商，无论是谷歌还是微软，都尝试着遵守相同的HTML规范，这使得网络编程能够在跨设备和不同操作系统间保持一致。

较低版本的IE浏览器没有遵循标准的HTML规范，所以我们需要做一些兼容处理来保证低版本IE浏览器正常工作。请查看 Internet Explorer章节获取详情信息。

当前，新的HTML标签已经开始出现，这些是HTML5规范的一部分。例如，Video标签，它指定了一个视频、影片剪辑或视频流

<pre><code>
<video href="/goofy-video.mp4"></video>
</code></pre>

这些新的HTML5标签在新的浏览器上工作，一般不支持IE8及以下版本。

### Directives: 自定义的HTML元素和属性
鉴于我们对HTML元素的认识，directives是Angular中创建新的拥有自定义功能的HTML元素的方法。例如，我们可以创建一个在所有浏览器中工作的自定义Video标签。

<pre><code>
<my-better-video my-href="/goofy-video.mp4">Can even take text</my-better-video>
</code></pre>

请注意，我们的自定义元素具有自定义起始和关闭标签，my-better-video和一个自定义属性：my-href。
为了使我们的标签更可用，我们可以复写浏览器提供的视频标签，这意味着我们可以改为使用：

<pre><code>
<video my-href="/goofy-video.mp">
Can still take children nodes
</video>
</code></pre>

就如我们看到的，指令可以结合其他指令和属性；该组合被称为构造。
为了更好的理解如何从小的部分组成一个系统，我们必须先了解原始件。熟悉和理解它们是后续章节的基本目标。
让我们开始吧。

### Bootstrapped HTML
当浏览器加载我们的HTML时且载入Angular，我们只需要一段代码启动我们的Angular应用（我们在 introductory chapter 章节已经学习过了）。
在我们的HTML中使用内置的ng-app指令标记我们的根节点。这个指令作为一个属性来使用，因此，我们可以把它插入任何地方，但我们选择< HTML >的超始标签，这是规范：
A built-in directive is one that ships out of the box with Angular.所有的内置指令以ng为命名空间前缀。为了避免命名空间冲突，请不要使用ng做前缀自定义指令名称。
 
<pre><code>
<html ng-app="myApp">
&lt;!-- $rootScope of our application --&gt;
</html>
</code></pre>

现在，在<html>元素里，我们可以使用任何我们想要的内置或自定义指令。 此外，在JavaScript代码中，在这个根元素范围内使用的所有指令都可以获取以$ rootScope作为原型继承的scope，该指令的方法可以访问此scope。当scope可以访问时，意味着scope已经与DOM链接完成，这个过程是在指令周期将要结束时完成的。

因为一个指令的生命周期是很复杂的，需要做为一节来介绍。在这一节中，我们还将讨论指令中的哪些方法可以访问scope以及指令中如何共享scope。查看directives explained章节获取更多信息。

### 首个指令
让我们的湿脚的最快方法是迈进水里。让我们继续前进，实现一个简单的自定义指令。稍后我们将会定义如下的HTML元素：

<pre><code>
<my-directive></my-directive>
</code></pre>

前提是我们已经创建了一个HTML文档，并引入了Angular，
而且在我们的应用中，已将ng-app指令标记在了DOM的根元素上，当Angular编译我们的HTML时，会调用这个指令。
	我们将进一步了解指令周期的编译过程在understanding compile章节。

调用指令意味着执行我们通过指令定义的JavaScript。
myDirective指令定义如下所示：

<pre><code>
angular.module('myApp', [])
.directive('myDirective', function() {
return {
restrict: 'E',
template: '<a href="http://google.com">Click me to go to Google</a>'
}
});
</code></pre>


