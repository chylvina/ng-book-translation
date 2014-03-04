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

AngularJS很容易创建自己的Services：首先我们需要去注册一个service。当service被注册后，Angular的编译器在运行时通过依赖关系去引用加载它。这种名字注册方式有效的降低了测试中进行mocks和stubbing的藕合度。

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

githubService并没有做有意义的事情，但是它已经被AngularJS注册了，应用可以通过githubService作为它的名字来用它。

factory()函数可以把一个单例对象或者函数转化成一个service，并存在在应用的生命周期里。当我们的Angular加载这个service，这个service将执行这个函数返回一个单例的service对象。

factory()函数可以是一个函数或者是一个数组，就像我们创建controllers:

<pre><code>

// 用bracket notation创建factory

angular.module('myApp.services', []).factory('githubService',[function($http) {

}]);

</code></pre>

例如，githubService需要$http，所以我们需要在参数中列出$http注入到函数中。

<pre><code>
angular.module('myApp.services',[]).factory('githubService', function($http) {

  //现在我们的service实例，已经把 $http以参数形式引入函数

  var serviceInstance = {};

  return serviceInstance;

});
</code></pre>

现在，无论在任何地方我们需要调用GitHub API，我们再也不用通过$http，我们可以通过githubService来代替去做处理复杂的远程服务。

GitHub API会给GitHub的用户暴露一些接口(这些接口会提供一些这个用户最近登录到GitHub的一些行为)。在我们的这个service里，我们会创建一个方法访问这个API然后将放回结构传给我们的API。

我们可以在service中通过返回对象的属性去暴露一个方法。

<pre><code>

angular.module('myApp.services', []) .factory('githubService', function($http) {

  var githubUrl = 'https://api.github.com';

  var runUserRequest = function(username, path) {

    //通过 $http service 返回JSONP格式的 Github API

     return $http({

        method: 'JSONP',

        url: githubUrl + '/users/' +

              username + '/' +

              path + '?callback=JSON_CALLBACK'

     });
  }

    // 返回一个service对象，里面有一个events函数

  return {

    events: function(username) {

      return runUserRequest(username, 'events');

    }

  };

});
</code></pre>

githubService返回的这个方法，我们可以以组件形式在我们的应用中调用。

Using Services(使用Services)

在我们需要使用一个组件时，将service作为它的依赖来用：一个controller, 一个directive,一个filter, 或者另外一个 service。在运行的时，Angular会实例化它，并且处理好它的依赖关系。

我们用service的名字作为入参到controller函数里，将这个service注入到controller里。通过controller的依赖关系，我们可以使用service的方法。

<pre><code>

angular.module('myApp', ['myApp.services']).controller('ServiceController',

  function($scope, githubService) {

    //我们可以通过这个对象调用events 这个函数

    $scope.events = githubService.events('auser');

});
</code></pre>

githubService注入我们的ServiceController，它使用起来就像另外的一个server。

让我们写一个例子用我们view中定义用户名去调用GitHub API。我们会用数据绑定部分的内容，将用户名绑定到这个view上。

<pre><code>

&lt;div ng-controller="ServiceController"&gt;

  &lt;label for="username"&gt;

      Type in a GitHub username

  &lt;/label&gt;

  &lt;input type="text" ng-model="username" placeholder="Enter a GitHub username" /&gt;

  &lt;ul&gt;

    &lt;li ng-repeat="event in events"&gt;

    &lt;!-- {{ event | json }} --&gt;

    {{ event.actor.login }} {{ event.repo.name }}

    &lt;/li&gt;

  &lt;/ul&gt;

&lt;/div&gt;

</code></pre>

现在我们可以通过监听$scope.username属性，基于双向绑定做出页面的变化。