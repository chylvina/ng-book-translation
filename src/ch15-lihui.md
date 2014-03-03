#Services(服务)
直到现在，我们只是关心我们的$scope如何关联View的和controller如何管理数据的。由于考虑到内存和性能的，controllers只会在用的时候被实例化，再不用的时候又会被销毁掉。这就意味着每次我们切换route或者重新加载一个view，当前的controller会被Angular清除掉。

Services给我们提供一个方法在保存数据在app的生命周期里并且可以使数据可以在多个controller间传递。

Services是单例对象只会在每一个App里（被$injector）实例化一次，然后被懒加载（当我们需要它的时候）。它提供一个接口把多个方法汇总在一起放在一个特定的函数里。

例如$http就是AngularJS的一个Service。它提供了一个简单的接近于浏览器的XMLHttpRequest对象。我们可以很方便的通过$http去获取XMLHttpRequest对象，而不需要自己麻烦的去写一个应用给我们提供XMLHttpRequest对象。

<pre><code>
//一个service的例子 current_user 将会在一直存在在app的生命周期里
angular.module('myApp',[]).factory('UserService', function($http) {
  var current_user;
  return {
    getCurrentUser: function() {
      return current_user;
    },
    setCurrentUser: function(user) {
      current_user = user;

    }
  }
});
</code></pre>

AngularJS给我们提供了一些内置的Services使我们可以在全局里交互。它们还能在我们自己写的Services里起到很重要的作用。

AngularJS很容易创建自己的Services：首先我们需要去注册一个service。当service被注册后，Angular的编译器在运行时通过依赖关系去引用加载它。The name registry makes it easy to isolate application dependencies for mocks and stubbing in our tests.

#Registering a Service(注册一个Service)
通过$injector有几种方式去创建和注册一个Service，我们会在后边的章节给大家讲解。

通过angular.module的factory方法去创建一个service是最常见和便捷的：

<pre><code>
angular.module('myApp.services', []).factory('githubService', function() {
  var serviceInstance = {};
  // 我们的第一个service
  return serviceInstance;
});
</code></pre>

githubService并没有太大的实际意义，但是它已经被AngularJS注册了，应用可以通过githubService作为它的名字来用它。

