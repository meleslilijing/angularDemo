# AngularDemo 
学习Angularjs

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

