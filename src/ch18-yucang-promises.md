##Promises	Angular’s event system (which we discuss in depth in the under the hood chapter) provides a lot of power to our Angular apps. One of the most powerful features it gives us is the automatic resolution of promises.
angular的事件系统(我们已经在under the hood chapter一章中深入讨论过)为angular应用提供了很多有用的特性.automatic resolution of promises便是其中最有用的特性之一.
####What’s a Promise?	A promise is a method of resolving a value (or not) in an asynchronous manner. Promises are objects that represent the return value or thrown exception that a function may eventually provide. Promises are incredibly useful in dealing with remote objects, and we can think of them as a proxy for our remote objects.

Promise是一个解决异步行为中数据如何处理问题的方法.Promises是表示一个函数最终产生的返回值或错误处理的对象.Promises在处理remote objects方面有难以置信的优势,可以认为是我们remote objects的代理对象.	Traditionally, JavaScript uses closures, or callbacks, to respond with meaningful data that is not available synchronously, such as XHR requests after a page has loaded. Rather than depending upon a callback to fire, we can interact with the data as though it has already returned.

一般来说,JavaScript利用闭包或者回调函数(Callbacks)去处理同步请求获取不到的数据,例如页面加载后的XHR请求等等.与触发回调函数的方式相比,用Promise方式处理数据时可以假设数据已经返回完成.	 Callbacks have worked for a long time, but the developer suffers when using them. Callbacks provide no consistency and no guaranteed call, they steal code flow when depending upon other callbacks, and they generally make debugging incredibly difficult. At every step of the way, we have to deal with explicitly handling errors.

回调函数已经被使用了很长时间,但是开发者在使用他们的过程中不得不忍受很多不便.回调函数提供的是非一致性和无保证的执行过程,当依赖其他回调函数时会截断当前代码流.他们还会使调试变得异常艰难.在每一个回调函数的处理过程中,我们都要明确的处理异常.	Instead of firing a function and hoping to get a callback run when executing asynchronous methods, promises offer a different abstraction: They return a promise object.
为了取代执行异步操作触发一个函数然后期待获得一个callback的方式,现在promises提供了完全不同的方式: 异步操作返回一个promise对象.	For example, in traditional callback code, we might have a method that sends a message from one user to one of the user’s friends.

例如,我们有一个方法,用传统回调函数的代码方式,从一个用户发送信息到该用户的朋友:
<pre><code>
// Sample callback codeUser.get(fromId, {  success: function(err, user) {	if (err) return {error: err}; 
	user.friends.find(toId,function(err, friend) {	  if (err) return {error: err};      user.sendMessage(friend, message, callback);    });  },  failure: function(err) {  return {error: err} }});</code></pre>

	This callback pyramid is already getting out of hand, and we haven’t included any robust error- handling code, either. Additionally, we need to know the order in which the arguments are called from within our callback.
	
这个回调层叠已经无法掌控,而且我们还没有加入健壮的异常处理代码.此外,我们还需要知道在我们回调函数中使用的参数的顺序.
	The promised-based version of the previous code might look somewhat closer to:promise版本的代码看起来像是下面这样:
<pre><code>User.get(fromId) .then(function(user) {return user.friends.find(toId); }, function(err) {  // We couldn't find the user}) .then(function(friend) {return user.sendMessage(friend, message); }, function(err) {  // The user's friend resulted in an error}) .then(function(success) {  // user was sent the message}, function(err) {// An error occurred});</code></pre>	Not only is this code more readable; it is also much easier to grok. We can guarantee that the callback will resolve to a single value, rather than having to deal with the callback interface.
这段代码不仅可读性更高,而且更容易理解.我们可以保证回调会resolve to a single value,而不是去having to deal with the callback interface.
	Notice that in the first example, we have to handle errors differently from how we handle non-errors. We need to make sure when using callbacks to handle errors, we check if an error is defined in the tradition API response signature (usually with (err, data) being the usual method signature). All of our API methods must implement this same structure.

注意在第一个例子中,我们需要处理异常的与不处理异常的部分代码十分不同.在使用回调方式中处理异常,我们需要确保我们已经检查过异常是否在规范API的response signature中已经定义(通常用(err, data) being the usual method signature).All of our API methods must implement this same structure.In the second example, we handle the success and error in the same way. Our resultant object will receive the error in the usual manner. The promise API is specific about resolving or rejecting promises, so we also don’t have to worry about our methods implementing a different method signature.