# Vue.js Lesson 1

> vue.js 到底是什么？我觉得这个不用我来说，可以直接给[链接](https://cn.vuejs.org/v2/guide/)，在官网就说得很明白了。

## 引言

其实一般人都知道，关于Vue.js的教程在网上一搜一大把，而且官网本身资源和说明也挺足够的。

不过我最近的朋友遇到了一些问题，他没办法根据官网的内容去完成一个又一个的demo。或者说他们缺乏SPA(single page applcation)的开发经验，看完了教程还是不懂怎么使用。

这样子，额...不如我来写一篇真正意义上面向新手的教程吧，或者说是面向真正的零基础的人员的教程。

当然，我说的零基础是没有任何SPA开发经验和没有工程化项目经验的人。你还是得知道最基础的Javascript、HTML、CSS，不然一切就免谈了。

至于我认为需要什么样的基础，给出一些建议

基础:

* [HTML + CSS 基础](https://www.imooc.com/learn/9)
* [Javascript 基础](https://www.imooc.com/learn/36)
* [DIV/CSS 布局](https://www.zhihu.com/question/35865205)

进阶:
* [SASS](https://www.sass.hk/docs/)：这个后面补充一句，我觉得可以用[Koala](http://koala-app.com/index-zh.html)，在我们还不懂怎么通过`webpack`、`gulp`之类的东西去编译的时候。
* [ECMAScript 6](http://es6.ruanyifeng.com/)：因为在vue的学习过程中会看到很多什么钩子函数之类的，如果不懂看起来是挺幸苦的，ES6做的事是在扩充Javascript，所以这个遇到再去查也是可以的。

## 如何使用 Vue.js

很多真正的新手，其实从这里开始就已经懵逼，为何？

虽然在官网上给了足够的提示告诉用户怎么去使用vue.js。但是因为他们都是碎片的，没有整合到一文件里面，因为很多新手无法从把握它的全貌。

我们可以先不去管什么是`vue-cli`，我们先从最原始的引入方法，通过`<script>`标签引入我们的vue.js。

我们可以在尝试vue.js的教程时候完全可以，把他们写在单独的html里面。

当然后面还是会介绍如何使用`vue-cli`去构建我们vue.js的工程化的项目

## Hello Vue.js

起步要做什么，肯定就是程序员日常的`Hello World`了。我们可以根据官方最简单的例子用 vue.js 去编写一个 `Hello vue.js`

废话不多说直接上代码，现在我把代码整合到一个html文件里面

```html
<body>
  <script src="https://cdn.jsdelivr.net/npm/vue"></script>
  <div id="app">
    {{ msg }}
  </div>
  <script>
    var app = new Vue({
      el: '#app',
      data: {
        msg: 'Hello Vue.js'
      }
    })
  </script>
</body>
```

[在线例子](https://jsfiddle.net/JefferyLiang/x75tkdap/)

对应的html文件放在当前文件夹下的`/demo/lesson1-1.html`

这样子我们的第一个Vue应用就已经运行起来了。在页面上能看到`Hello vue.js`几个字。

### 详细说明

这里我们可以看到这样的一段代码

```javascript
var app = new Vue({
  el: '#app',
  data: {
    msg: 'Hello Vue.js'
  }
})
```

这一段Javascript代码做的事就是把我们引入的vue.js是实例化

里面相关的参数:

* el: vue.js应用挂载到对应的dom上，如`#app`则是挂在到id是`app`的dom上
* data: 定义vue应用可响应数据，我们在上面的例子定义了一个`msg`的变量。当变量发生改变时候，在vue.js的应用内的`{{ msg }}`数据也回随之变化。具体详细说明可以看[这里](https://cn.vuejs.org/v2/api/#data)


但是，这看起来一点都不 Cool 对不对，因为我们完全可以

```html
<div>
  Hello Vue.js
</div>
```

这样子来实现，而且还少写了很多代码，看起来是这样子。

## 响应式渲染页面

我们刚写了一个一点都不 Cool 的 Hello Vue.js 页面。不过那里例子我们只是运行了vue.js。我们现在来看看他的响应式渲染页面。

响应式渲染页面，最通俗的理解就是当数据改变，我们的页面内容回随着数据的变化一同改变。

现在我们修改一下上面的html文件,如下

```html
<body>
  <script src="https://cdn.jsdelivr.net/npm/vue"></script>
  <div id="app">
    <div>
      <span>{{ msg }}</span>
    <div>
    <input type="text" v-model="msg"/>
  </div>
  <script>
    var app = new Vue({
      el: '#app',
      data: {
        msg: 'Hello vue.js'
      }
    })
  </script>
</body>
```

[在线例子](https://jsfiddle.net/JefferyLiang/7u2z7t5j/)

对应html文件位置`/demo/lesson1-2.html`

在我们加入了一个`input`标签之后，我们发现当我更改我们输入框的内容时，我们之前`Hello Vue.js`的文字也会随之改变。这就是vue.js的响应式渲染页面了。

我们仅仅添加了一个`input`标签，并且在标签上用`v-model`把数据`msg`的内容绑定到input上。这样子我们能够通过修改输入框的内容去修改`data`里面的`msg`的数据，当vue.js检测到`msg`修改了，就去重新渲染我们的页面，让我们看到的span里面的的文字也随之修改。

这就是Vue.js的响应式渲染。想象一下以前同Javascript更新DOM是多麻烦。

## 结语

这一节简单的介绍了一下 vue.js 是什么，还有他最大的一个特点响应式渲染。同时用最简单的的方法引入并使用了 vue.js。

当然vue.js的能做的事不仅仅是这些，我们会在和面一一给各位介绍，如：列表渲染、条件渲染、计算属性、过滤器等等一系列东西。

当我们完成了解了vue.js又什么特点，就会开始使用`vue-cli`构建工程化项目，在项目之中使用vue.js。

时间: 2018-03-27

作者: Jeffery Liang

邮箱: lzh.jeffery@gmail.com

如果要转载请通过邮箱联系我。并声明原文地址。