# Vue Lesson 4

> 这一节介绍关于计算属性，这个东西是 vue.js 比较特别的一个东西。

## 引言

上一节，我们了解了 vue.js 的生命周期的事情，这一节我们介绍的是关于 `计算属性` 的事情。

至于什么是计算属性呢？

就是能够根据我们在方法里的 `data` 域的数据变化而变化的一个变量，我们不如直接下去看一下吧。

## 计算属性

我们先去使用计算属性去试一下，有什么神奇的的地方

```html
<body>
  <div id="app">
    <p>原字符: {{ msg }}</p>
    <p>计算字符: {{ reversedMessage }}</p>
  </div>
  <script>
    let app = new Vue({
      el: '#app',
      data: {
        msg: 'Hello Vue.js'
      },
      computed: {
        reversedMessage () {
          return this.msg.split('').reverse().join('')
        }
      }
    })
  </script>
</body>
```

[在线例子](https://jsfiddle.net/JefferyLiang/bg8tff65/)
文件位置: `/demo/lesson4-1.html`

## 侦听器

关于计算属性的，我们可以看到我们把的数据进行一些列操作之后，返回回来。但是这个例子我们还没看到计算属性真正神奇的地方。我们可以对上面的例子进行简单的修改，就能够看到 `计算属性` 的神奇的地方

```html
<body>
  <div id="app">
    <p>原字符: {{ msg }}</p>
    <p>计算字符: {{ reversedMessage }}</p>
  </div>
  <script>
    let app = new Vue({
      el: '#app',
      data: {
        msg: 'Hello Vue.js'
      },
      computed: {
        reversedMessage () {
          return this.msg.split('').reverse().join('')
        }
      },
      created () {
        setTimeout(() => {
          this.msg = 'This is new String'
        })
      }
    })
  </script>
</body>
```

[在线例子](https://jsfiddle.net/JefferyLiang/pf6eb0jv/5/)
文件位置: `/demo/lesson4-2.html`

### 详细说明

我们通过 `created` 钩子，在2秒后改变我们的 `msg` 变量。这时候我们就看到了计算属性的神奇的地方了。

我们能看到当 `msg` 发生改变的同时，`reversedMessage` 这个计算属性也随之发生变化了。这就是计算属性的厉害的放。

它能够侦测到与其相关的 `data` 的变量，并且在它发生改变的时候，随之改变。

## 比较

当然从上面的例子，其实能实现这种效果除了 计算属性 还有别的方法可以实现。

### 函数

我们能够把计算属性中的逻辑封装到一个方法之中，这样子我们能实现相同的效果，但是他们还是有所区别的。

在计算属性之中，会把结果进行缓存，这样子只要依赖的数据不发生改变，我们能直接访问这个值。

在方法之中，我们每次都要运行这个方法，然后返回这个值，效能上会有所差别。而且当组件刷新时候，我们也要重新的执行方法去获取。

### $watch

在 vue.js 里还有跟 ng 类似的一个 `$watch` 的设置。

我们可以看下面的这一段代码

```html
<div id="app">
  {{ fullName }}
</div>
<script>
  let app = new Vue({
    el: '#app',
    data: {
      firstName: 'Foo',
      lastName: 'Bar'
      fullName: ''
    },
    watch: {
      firstName: function (val) {
        this.fullName = `${val} ${this.lastName}`
      },
      lastName: function (val) {
        this.fullName = `${this.firstName} ${val}`
      }
    }
  })
<script>
```

上面这一段代码我们能够通过 `$watch` 去实现当 `firstName` 或者 `lastName` 变化时候，去更新 `fullName` 但是却略显麻烦。

下面我们来看看使用 计算属性 怎么实现

```html
<div>
  {{ fullName }}
</div>
<script>
  let app = new Vue({
    el: '#app',
    data: {
      firstName: 'Foo',
      lastName: 'Bar'
    },
    computed: {
      fullName () {
        return this.firstName + ' ' + this.lastName
      }
    }
  })
</script>
```

这样子的实现方法是不是感觉就舒服多了。

当然如果你不想数据被缓存的话，那就别使用 计算属性 了。

## 结语

当然 计算属性 还有一些可以用的地方，不过碍于篇幅我就不多说了，官方的[教程](https://cn.vuejs.org/v2/guide/computed.html)其实说得很详细的。

我们使用 vue.js 的记得活用 计算属性 ，后面我还会写一遍关于 vue.js 是如何侦查 计算属性 的依赖数据的文章，这个可以通过解读源码看到，别人的用什么精巧的方法去处理这些问题。