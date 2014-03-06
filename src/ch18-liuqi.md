##Promises



Promise是一个处理异步数据的方法.Promises对象代表一个函数最终产生的返回值或异常处理.Promises在处理远程对象调用方面十分有用,可以被看作远程对象的代理.

一般来说,JavaScript使用闭包或者回调函数(Callbacks)去处理异步请求的数据,比如页面加载完成后的XHR请求等.而与回调函数方式相比,Promises方式可以像数据已经返回一样处理异步数据.


回调函数已经使用了很久,但是开发者不得不忍受它的很多不便.回调函数提供的是非一致性和无保证的执行过程,当依赖其它回调函数时会截断当前代码执行.回调函数还使调试过程变得异常艰难.并且使用回调函数方式的每一步,我们都要明确的处理异常.

取代了回调函数方式(执行异步操作,触发一个函数然后等待返回一个回调),Promises使用不同的方式: 异步操作返回一个promise对象.

例如,我们有一个方法,使用传统回调函数的代码方式,作用是从一个用户发送信息到该用户的朋友:
<pre><code>
// Sample callback code
// 回调函数方式示例代码
	user.friends.find(toId,function(err, friend) {

This callback pyramid is already getting out of hand, and we haven’t included any robust error- handling code, either. Additionally, we need to know the order in which the arguments are called from within our callback.
	
这个回调嵌套已经非常复杂,而且我们还没有加入健壮的异常处理代码.不仅如此,我们还需要知道我们在回调函数中使用的参数的顺序.

The promised-based version of the previous code might look somewhat closer to:
<pre><code>
  // 给user的朋友发送消息失败



注意在第一段代码示例中,处理异常与不处理异常部分的代码十分不同.在使用回调方式中处理异常,我们需要确保我们已经检查过异常是否在API的响应方法签名中已经定义(通常用(err, data) 作为方法签名).所有的API方法必须实现这个相同的结构.

