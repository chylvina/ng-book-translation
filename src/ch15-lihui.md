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

<pre><code>

.controller('ServiceController',function($scope, githubService) {

  // 监听username的变化，当它改变的时候，执行下面的函数

  $scope.$watch('username', function(newUsername) {

    //用$http  service 去请求GitHub API，再返回结果

    githubService.events(newUsername).success(function(data, status, headers) {

      // 成功时由一个函数包裹,相应的数据在data里

      // 所以我们需要通过 data.data 去获取数据

      $scope.events = data.data;

     });

  });

});


</code></pre>

应为我们返回了$http的promise，我们可以通过.success()方法直接调用$http。

在controller 里使用$watch 是不推荐的。我们只是作个简单的示范。在真实操作中，我们会把$watch放在directive中去代替。

在这个例子中，我们会注意到输入框变化有延时机制。如果我们不引入延时机制，我们会在每次输入都会向GitHub API发起请求，这样就不是我们想要的。

我们用内置的 $timeout 这个service来解释延时机制。我们使用$timeout，会像githubService注入进controller那样把$timeout注入进去

<pre><code>

app.controller('ServiceController', function($scope, $timeout, githubService) {

});

</code></pre>

一般我们会把Angular的内置的service 放在我们自己定义的service 之前。

现在我们可以在controller里用$timeout。$timeout 在这种情况下会取消350ms内对输入框的操作，从而取消这些网络请求。换句话说，如果我们在输入时超过350ms，会认为操作完成并向 GitHub 发起请求。

<pre><code>

app.controller('ServiceController', function($scope, $timeout, githubService) {

  // 像上面的例子一样 优先注入$timeout

  var timeout;

  $scope.$watch('username', function(newUserName) {

  if (newUserName) {
    // 如果这里已经 有了一个timeout

    if (timeout) $timeout.cancel(timeout);
    timeout = $timeout(function() {

    githubService.events(newUserName).success(function(data, status) {

        $scope.events = data.data;

      });

    }, 350);

    }

  });

});

</code></pre>

至此，我们只是在关注service如何包裹一个函数。我们同样可以用service去包裹数据对象，在controller里调用。

例如，如果我们的应用程序要从后端服务需要身份验证，我们可能希望创建一个SessionsService处理用户身份验证，并持有到由后端服务通过一个令牌。当任何部分我们的应用程序要作出一个身份验证的请求，它可以使用SessionsService来获得访问令牌。

如果我们的应用程序中都有设置用户的GitHub的用户名的设置页面，我们会想，在我们的应用程序中的其它controller共享的用户名。

我们需要一个方法添加到我们的service去分享跨controller存储的用户名。记住，service 是一个单例的服务，所以我们可以安全地存储用户名。

<pre><code>

angular.module('myApp.services', []) .factory('githubService', function($http) {

  var githubUrl = 'https://api.github.com',

        githubUsername;

  var runUserRequest = function(path) {

  //通过$http 返回 JSONP 格式的Github API

    return $http({

      method: 'JSONP',

      url: githubUrl + '/users/' + githubUsername + '/' +  path + '?callback=JSON_CALLBACK'

      });
  }

  // 返回 service 对象 包含  events 和 setUsername 两个方法

  return {

    events: function() {

        return runUserRequest('events');

      },

    setUsername: function(username) {

        githubUsername = username;

      }

    };

  });


</code></pre>

现在，我们的service有了setUsername方法，使我们能够设置用户名当前GitHub的用户。

在我们的应用程序的任何controller ，我们可以注入的githubService并调用events()不需要关心是否有一个正确的username在scope对象里。

<pre><code>

angular.module('myApp', ['myApp.services']).controller('ServiceController',

  function($scope, githubService) {

    $scope.setUsername =  githubService.setUsername;

});


</code></pre>

#Options for Creating Services(创建Services的选项)

通常Angular注册一个 service 用的是factory() 方法，这里还有一些别的API，会帮我们减少注册一个 service的代码。

5种创建service的方法：

  * factory()
  * service()
  * constant()
  * value()
  * provider()

##factory()

factory()方法是一个快速的方法来创建和配置service。factory()函数有两个参数：

  * name (string)

  注册的service的名称。

  * getFn (function)

  这个函数会在angular创建service时执行。

<pre><code>

angular.module('myApp') .factory('myService', function() {

  return {

    'username': 'auser'

  }

});

</code></pre>

因为service是一个单例对象getFn方法会在应用的生命周期被调用一次。当我们定义我们的service，getFn 可以用一个函数或者一个数组的方式将其他service 注入进来。

The getFn function can return anything from a primitive value to a function to an object (similar to the value() function).

<pre><code>

angular.module('myApp').factory('githubService', [

  '$http', function($http) {

    return {

      getUserEvents: function(username) {

        }

    }

}]);

</code></pre>

##service()
如果我们要使用一个构造函数来注册一个服务的实例，我们可以使用service()方法，这使我们能够注册一个构造函数为我们的service对象。

service()方法有两个参数：

  * name (string)

  我们需要要注册的服务实例的名称。

  * constructor (function)

  构造函数。

  service()函数要new关键字实例化。

<pre><code>

var Person = function($http) {

  this.getName = function() {

    return $http({

      method: 'GET',

      url: '/api/user'

    });

  };

};

angular.service('personService', Person);

</code></pre>

##provider

这些factories 都通过$provide的服务创建的，它负责在运行时实例化这些provider。

一个provider就是一个有$get()方法的对象。$injector调用$get方法创建一个service实例。在$provider提供了几个不同的API方法创建一个service，每一个不同的预期用途。

在所有的方法来创建一个service的根源通过是provider的方法。provider()方法负责在$providerCache注册服务。

技术上，factory()函数是简写通过provider()方法创造一个service，其中$get()函数是传入的函数。

这两个方法调用功能相同，将创建相同的服务。