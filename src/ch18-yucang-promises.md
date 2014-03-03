##Promises



Promise是一个解决异步行为中数据如何处理问题的方法.Promises是表示一个函数最终产生的返回值或错误处理的对象.Promises在处理remote objects方面有难以置信的优势,可以认为是我们remote objects的代理对象.

一般来说,JavaScript利用闭包或者回调函数(Callbacks)去处理同步请求获取不到的数据,例如页面加载后的XHR请求等等.与触发回调函数的方式相比,用Promise方式处理数据时可以假设数据已经返回完成.

回调函数已经被使用了很长时间,但是开发者在使用他们的过程中不得不忍受很多不便.回调函数提供的是非一致性和无保证的执行过程,当依赖其他回调函数时会截断当前代码流.他们还会使调试变得异常艰难.在每一个回调函数的处理过程中,我们都要明确的处理异常.


例如,我们有一个方法,用传统回调函数的代码方式,从一个用户发送信息到该用户的朋友:
<pre><code>
// Sample callback code
	user.friends.find(toId,function(err, friend) {

	This callback pyramid is already getting out of hand, and we haven’t included any robust error- handling code, either. Additionally, we need to know the order in which the arguments are called from within our callback.
	
这个回调层叠已经无法掌控,而且我们还没有加入健壮的异常处理代码.此外,我们还需要知道在我们回调函数中使用的参数的顺序.

<pre><code>



注意在第一个例子中,我们需要处理异常的与不处理异常的部分代码十分不同.在使用回调方式中处理异常,我们需要确保我们已经检查过异常是否在规范API的response signature中已经定义(通常用(err, data) being the usual method signature).All of our API methods must implement this same structure.