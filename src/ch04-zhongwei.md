#Modules(模块)  
在javascript项目中，把功能性的代码放在全局命名空间中向来不是一个好习惯。这种方式可能会造成代码冲突，难以调试，会浪费我们宝贵的开发时间。  
在上一章中我们学习了数据绑定，我们是通过下面的代码在全局命名空间中定义独立的函数的方式实现的：  
<pre>
<code>
function MyController($scope){
    var updateClock = function(){
        $scope.clock = new Date();
    }
    setInterval(function(){
        $scope.$apply(updateClock);
    },1000);
    updateClock();
};
</code>
</pre>
  
在本章中，我们将会讨论如何编写高效的、可部署的controllers，我们将这种封装功能性的代码的模块单元称为module。  
在Angular中，module(模块)是构建AngularJs app的主要方式。AngularJs app的module将包含我们项目中的所有代码，一个app可能包含很多modules，每一个module会实现不同的功能。  
使用modules的方式给我带来很多好处，例如：  

   * 不会污染全局命名空间
   * 由于每个module针对不同的功能性代码，所以便于构建测试、维护
   * 便于在不同的应用中共享代码
   * 可以在app中按任意的顺序载入不同的代码   
    
Angular module API提供了angular.module()方法，用于声明一个module。声明一个module，需要两个参数，第一个参数是我们构建的module的名称；第二个参数是当前module的所依赖的元素列表，就是所谓的依赖注入
<pre>
<code>
angular.module('myApp',[]);
</code>
</pre>
#####Modules
<img style="display:block;float:left;"src="http://ringtail.u.qiniudn.com/ng-book-00"/>
<p style="padding-top:25px;margin-left:30px;">这个方法是Angular module的<i>setter</i>方法,我们可以通过这种方式定义模块<p>  
我们也可以通过只传递module名称的方式，用这个方法来引用module，例如我们可以像这样来引用myApp module:  
<pre>
<code>
//这个方法获得了myApp的一个引用
angular.module('myApp')
</code>
</pre>
<img style="display:block;float:left;"src="http://ringtail.u.qiniudn.com/ng-book-00"/>
<p style="padding-top:25px;margin-left:30px;">这个方法就是所谓的<i>getter</i>方法,我们可以通过这种方式引用模块<p>  
到目前为止，我们就可以在angular.module('myApp')之上构建我们的应用了。  
构建大型的应用，我们可能需要创建很多不同的模块来实现复杂的业务逻辑。将不同的业务逻辑进行抽离，封装为不同的module，可以使我们的更容易进行测试。想要获得更多关于如何编写高可用的module,请参考architecture 章节
###Properties
Angular module有很多用来区分不同module的属性
####name(string)
modules的的name属性会返回一个字符串，代表当前module的名字
####requires（array of string)
requres属性是一个字符串数组，表示当前模块的依赖注入，这些依赖注入会在module运行之前进行调用。
