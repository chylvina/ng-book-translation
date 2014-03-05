##PromisesAngular’s event system (which we discuss in depth in the under the hood chapter) provides a lot of power to our Angular apps. One of the most powerful features it gives us is the automatic resolution of promises.
Angular的事件机制(我们已经在`under the hood`一章中深入讨论过)为angular应用提供了许多有用的特性.`Promises自动解决方案`是其中最有用的特性之一.
###What’s a Promise?A promise is a method of resolving a value (or not) in an asynchronous manner. Promises are objects that represent the return value or thrown exception that a function may eventually provide. Promises are incredibly useful in dealing with remote objects, and we can think of them as a proxy for our remote objects.

Promise是一个处理异步数据的方法.Promises对象代表一个函数最终产生的返回值或异常处理.Promises在处理远程对象调用方面十分有用,可以被看作远程对象的代理.Traditionally, JavaScript uses closures, or callbacks, to respond with meaningful data that is not available synchronously, such as XHR requests after a page has loaded. Rather than depending upon a callback to fire, we can interact with the data as though it has already returned.

一般来说,JavaScript使用闭包或者回调函数(Callbacks)去处理异步请求的数据,比如页面加载完成后的XHR请求等.而与回调函数方式相比,Promises方式可以像数据已经返回一样处理异步数据.
Callbacks have worked for a long time, but the developer suffers when using them. Callbacks provide no consistency and no guaranteed call, they steal code flow when depending upon other callbacks, and they generally make debugging incredibly difficult. At every step of the way, we have to deal with explicitly handling errors.

回调函数已经使用了很久,但是开发者不得不忍受它的很多不便.回调函数提供的是非一致性和无保证的执行过程,当依赖其它回调函数时会截断当前代码执行.回调函数还使调试过程变得异常艰难.并且使用回调函数方式的每一步,我们都要明确的处理异常.Instead of firing a function and hoping to get a callback run when executing asynchronous methods, promises offer a different abstraction: They return a promise object.

取代了回调函数方式(执行异步操作,触发一个函数然后等待返回一个回调),Promises使用不同的方式: 异步操作返回一个promise对象.For example, in traditional callback code, we might have a method that sends a message from one user to one of the user’s friends.

例如,我们有一个方法,使用传统回调函数的代码方式,作用是从一个用户发送信息到该用户的朋友:
<pre><code>
// Sample callback code
// 回调函数方式示例代码User.get(fromId, {  success: function(err, user) {	if (err) return {error: err}; 
	user.friends.find(toId,function(err, friend) {	  if (err) return {error: err};      user.sendMessage(friend, message, callback);    });  },  failure: function(err) {  return {error: err} }});</code></pre>

This callback pyramid is already getting out of hand, and we haven’t included any robust error- handling code, either. Additionally, we need to know the order in which the arguments are called from within our callback.
	
这个回调嵌套已经非常复杂,而且我们还没有加入健壮的异常处理代码.不仅如此,我们还需要知道我们在回调函数中使用的参数的顺序.

The promised-based version of the previous code might look somewhat closer to:用Promise方式写同样功能的代码看起来像是下面这样:
<pre><code>User.get(fromId) .then(function(user) {return user.friends.find(toId); }, function(err) {  // We couldn't find the user  // 找不到user}) .then(function(friend) {return user.sendMessage(friend, message); }, function(err) {  // The user's friend resulted in an error
  // 给user的朋友发送消息失败}) .then(function(success) {  // user was sent the message  // 给user的朋友发送消息成功}, function(err) {// An error occurred});</code></pre>Not only is this code more readable; it is also much easier to grok. We can guarantee that the callback will resolve to a single value, rather than having to deal with the callback interface.
这段代码不仅可读性更高,而且更容易理解.我们可以保证回调会最终会处理单一的值,而不是与回调函数接口打交道.
Notice that in the first example, we have to handle errors differently from how we handle non-errors. We need to make sure when using callbacks to handle errors, we check if an error is defined in the tradition API response signature (usually with (err, data) being the usual method signature). All of our API methods must implement this same structure.

注意在第一段代码示例中,处理异常与不处理异常部分的代码十分不同.在使用回调方式中处理异常,我们需要确保我们已经检查过异常是否在API的响应方法签名中已经定义(通常用(err, data) 作为方法签名).所有的API方法必须实现这个相同的结构.
In the second example, we handle the success and error in the same way. Our resultant object will receive the error in the usual manner. The promise API is specific about resolving or rejecting promises, so we also don’t have to worry about our methods implementing a different method signature.
在第二段代码示例中,我们用相同的方式处理成功和异常结果.我们的表示结果的对象会用普通的方式接收异常.Promise API会去处理肯定(resolve)或者否定(reject)的promises,所以我们也不需要去关心我们的方法实现了不同的方法签名.