###XHR in Practice



同域策略是仅允许来自相同站点脚本的在页面上运行.浏览器通过比较两个页面的协议(scheme),主机名(hostname)和端口号(port number)来判断它们是否同域.浏览器对从不同的域发起的交互脚本有着严格的限制.

在通过XHR请求数据或者处理外部资源时,跨域资源共享(Cross Origin Resource Sharing,以下简写为CORS)经常是一件让人头痛的事情.

幸运的是,有几种方法可以在我们的应用里获取外部暴露的数据.我们来详细看看其中的两个方法和简述的第三种方法(需要更多的后端的支持):

- JSONP

JSONP is a way to get past the browser security issues that are present when we’re trying to request data from a foreign domain. In order to work with JSONP, the server must be able to support the method.



与使用XHR请求不同,JSONP是利用`<script>`标签发送GET请求来工作的.JSONP技术创建了`<script>`标签并且将它放在DOM中.当它出现在DOM中时,浏览器接下来请求在src标记中引用的脚本.

When the server returns the request, it surrounds the response with a JavaScript function invocation that corresponds to a request about which our JavaScript knows.

Angular provides a helper for JSONP requests using the $http service. The jsonp method of request through the $http service looks like:






当数据从支持JSONP的服务端传回时,它被包裹在Angular的angular.callbacks._0自动生成的匿名函数中.在这种情况下，GitHub的服务器会返回一些包裹在回调中的JSON，应答可能如下：




在使用JSONP时,我们需要意识到潜在的安全风险.首先,我们开放了我们的服务器,允许后端服务器调用我们应用中的任意javascript代码.


We can only use JSONP to send GET requests, since we’re setting a GET request in the `<script>` tag. Additionally, it’s tough to manage errors on a script tag. We should use JSONP sparingly and only with servers we trust and control.

我们只可以使用JSONP发送get请求,因为我们在`<script>`标签中设置了一个GET请求.此外,在一个script标签中处理异常是非常困难的.我们应该只在可以信赖和控制的服务器上谨慎的使用JSONP.








