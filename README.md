# AngularDemo 
学习Angularjs

ui-route：嵌套路由
angular-ui：angular的指令库

## angular快速上手

angular是前端的mvc框架。框架存在的意义在于将代码进行分层。

### angular 核心特性

1. mvc
2. 模块化
3. 指令系统
4. 数向双向绑定

单向数据绑定：template（模版） ＋ model（数据）。生成html标签，然后将其插入view（文档流）中。

双向数据绑定：template生成文档流后，更改view会更新model，更改model也会更改view。通常view中只有表单会常常更新，


## 搭建环境

### 前端开发环境：
nodejs 是基础
1. 代码编辑：sublime
2. 断点调试：chrome插件batarang
3. 版本管理：git
4. 代码合并和混淆：grunt-ugtify（混淆）、grunt-concat（合并）、grunt-watch（监控文件变化）
5. 依赖管理工具：
6. 单元测试工具：
7. 集成测试工具：	

## mvc

### 为什么要mvc：
1. 模块化和复用
2. 工程化开发，多人开发
3. 维护方便

### 前端mvc的困难：
1. 操作dom但代码必须等待整个页面全部加载完成
2. 多个js文件之间如果出现互相依赖，程序员必须自己解决。
3. js的原型继承给开发者带来了困难。没有现成的继承工具。

只用利用它原来的特性来绕开这些困难。

## 依赖注入－模块化

*路由、模块、依赖注入*

- angularjs的模块化实现
- 完整的项目结构
- 使用ngRoute进行视图之间的路由
- 一切都是从模块开始的
- ng官方推荐的模块切分方式是什么
- 模块之间的依赖应该怎么做？

一个功能一个模块

### 目录结构

	app						// app
		css
		framework			// 存放其他控件
		imgs
		js
			app.js			// 作为启动点的js
			controllers.js	// 控制器
			directives.js	// 指令
			filters.js		// 过滤器
			services.js		// 服务
			route.js		// 路由
			
		tpls				// 模版temples，即html片段
		
		index.html
	
	node_module				// npm模块

	package.json			 // npm配置
	
### 路由示例代码

	// 定义模块
	var app = angular.module('bookStoreApp', [
		bookStoreapp依赖的模块列表，angular有官方依赖模块，需要下载
	]);
	
	// 路由设置
	app.config(function($routeProvider) {
		$routeProvider.when('/hello', {			// 地址
			templateUrl: 'tpls/hello.html',		// 使用模版（tpl）
			controller: 'HelloCtrl'				// 使用的控制器（controller）
		}).when('/list', {
			templateUrl: 'tpls/bookList.html',
			controller: 'BookListCtrl'
		}).otherwise({
			redirectTo: '/hello'				// 没有地址匹配时，重定向的位置
		})
	
	});
	
### ng官方推荐的模块切分方式
		
	app
		
		controllers.js	// 控制器
		directives.js	// 指令
		filters.js		// 过滤器
		services.js		// 服务
		route.js		// 路由
			
			
> 1. 任何ng应用都是由控制器、指令、过滤器、服务、路由组成
> 2. 控制器、指令、服务、路由、过滤器分别放在一个模块里面（可借助grunt合并）
> 3. 用一个总的app模块作为入口点，它依赖其他所有模块

### 常用ng
- ng-app
- ng-controller
- ng-click
- ng-show
- ng-hide
- ng-class
- ngAnimate

## 路由

### 前端路由的基本原理

- 哈希#：单页面切换
- html5中新的history API
- 路由但核心是给应用定义“状态”
- 使用路由机制会影响到应用的整体编码方式（需要预先定义好状态）
- 考虑兼容性问题与“优雅降级”

## 搭建angular应用

- 界面原型设计
- 切分功能模块并建立目录结构
- 使用angular-ui和bootstrap编写ui（UIRouter、ngGrid、表单校验）
- 编写Controller
- 编写Service
- 编写Filter
- 单元测试和集成测试

## ng-controller之间通信

Angular的ng-controller就是一个作用域。

不同的controller都有一个$rootScope对象，所有的ng-controller都继承自ng-app。

需要在不同作用域之间通信

通信对象：
- event
- data

ng方法：
- $on：接收event与data。
- $emint：向父（parent controller）传递event与data。
- $broadcast：向子（child controller）传递event与data。
- 平级（bro controller）得不到值

示例代码:

html：

	<div ng-controller="ParentCtrl">                <!--父级-->

		<div ng-controller="SelfCtrl">              <!--自己-->
		
			<a ng-click="click()">click me</a>
			
			<div ng-controller="ChildCtrl"></div>   <!--子级-->
		
		</div>	
		
		<div ng-controller="BroCtrl"></div>         <!--平级-->
		
	</div>

js:

	app.controller('SelfCtrl', function($scope) {
		$scope.click = function() {
			$scope.$broadcast('to-child', 'child');
			$scope.$emit('to-parent', 'parent');
		}
	});
	
	app.controller('ParentCtrl', function($scope) {
		$scope.$on('to-parent', function(event, data) {
			console.log('ParentCtrl', data); //父级能得到值
		});
		$scope.$on('to-child', function(event, data) {
			console.log('ParentCtrl', data); //子级得不到值
		});
	});
	
	app.controller('ChildCtrl', function($scope) {
		$scope.$on('to-child', function(event, data) {
			console.log('ChildCtrl', data); //子级能得到值
		});
		$scope.$on('to-parent', function(event, data) {
			console.log('ChildCtrl', data); //父级得不到值
		});
	});
	
	app.controller('BroCtrl', function($scope) {
		$scope.$on('to-parent', function(event, data) {
			console.log('BroCtrl', data); //平级得不到值 
		});
		$scope.$on('to-child', function(event, data) {
			console.log('BroCtrl', data); //平级得不到值 
		});
	});

output:

     ParentCtrl child
     ChildCtrl parent
     
# 指令 directive

ng中一切都是指令，很多都是系统指令。如ng-mod，ng-repeat等

示例代码：

	angular.module("MyModule", [])
	.directive("directiveName", function() {
		return {
			require：‘^其他指令名’		
			scope: {},					
			restrict: "AEMC",			
			compile:function(){			
			
			},
			controller:function() {		
			
			},
			link: function(scope, element, attr, supermanCtrl){		
			
			
			},
		}
	})

指令加载阶段

1. 加载阶段：加载angular.js，找到ng-app，确认应用边界
2. 编译阶段：
	1. 遍历全部dom，找到所有指令
	2. 根据指令代码中的*template*、*replace*、*transcule*转换dom结构
	3. 如果存在compile函数则调用
3. 链接阶段：
	1. 对每一条指令运行link函数
	2. link函数：在模型和视图之间进行动态关联；一般用来操作DOM、绑定事件监听器。
4. 备注：
	1. 大多数情况只需要编写link就行
	2. compile函数只在编译结束后运行一次；对于指令的每个实例，link都会执行一次。
	

restrict: 匹配模式

- A：attribute 属性
- E：element 元素
- M：comment 注释
- C：class 类

例： hello指令
	
	element:
	<hello></hello>
	attr:
	<div hello></div>
	class:
	<div class="hello"></div>
	class:
	<!-- directive:hello -->
	<div></div>
	
template：


replace：


	
# 模版缓存

缓存：

	// 注射器加载完所有模块时，run会被执行一次
	myModule.run(function($templetcatch) {
		$templetcatch.put("hello.html", "<div>......</div>")
	})

调用：

	$templetcatch.get("hello.html")	
	