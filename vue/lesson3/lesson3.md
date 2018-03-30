# Vue Lesson 3

> 关于 vue.js 的生命周期

# 引言

上一节，很初步的了解了 vue.js 的渲染指令，这一节我们去看看它的生命周期的钩子。

生命周期的钩子我们以后会经常更它打交道，当我们需要初始化数据，进入页面去请求api获取数据等等工作，一般都通过钩子来实现了。

## 生命周期

那么我们来看一下 vue.js 的生命周期图

！[加载失败](https://cn.vuejs.org/images/lifecycle.png)

在 vue.js 的生命周期里面我们需要关注的几个关键的钩子

分别是一下的:

* beforeCreate(构建前)
* created(构建完成后)
* beforeMount(DOM挂载前)
* mounted(DOM完成挂载)
* beforeUpdate(data数据更新前)
* updated(data数据更细之后)
* beforeDestroy(销毁前)
* destroy(销毁后)

那我们去体验一下,它的生命钩子

```html
<body>
  <div id="app">
    {{ msg }}
  </div>
  <script>
    let app = new Vue({
      el: '#app',
      data: {
        msg: 'Hello vue.js'
      },
      beforeCreate () {
        console.log('| --- before create')
        console.log('|')
      },
      created () {
        console.log('|')
        console.log('| --- | --- created')
        console.log('|')
        setTimeout(() => {
          // trigger update
          this.msg = 'change vue.js'
        }, 2000)
        setTimeout(() => {
          // trigger destroy
          this.$destroy()
        }, 4000)
      },
      beforeMounted () {
        console.log('|')
        console.log('| --- | --- | --- before mount')
        console.log('|')
      },
      mounted () {
        console.log('|')
        console.log('| --- | --- | ---- mounted')
        console.log('|')
      },
      beforeUpdate () {
        console.log('|')
        console.log('| --- | --- | ---- before update')
        console.log('|')
      },
      updated () {
        console.log('|')
        console.log('| --- | --- | ---- updated')
        console.log('|')
      },
      beforeDestroy () {
        console.log('|')
        console.log('| --- | --- before destroy')
        console.log('|')
      },
      destroyed () {
        console.log('|')
        console.log('| --- destroy')
      }
    })
  </script>
</body>
```

[在线例子](https://jsfiddle.net/JefferyLiang/snuwgwmL/)
文件位置: `/demo/lesson3-1.html`

### 详细说明

当我们运行上面的例子时候我们会在浏览器的控制台看到一下的输出

```javascript
// => | --- before create
// => |
// => | --- | --- created
// => |
// => | --- | --- | --- before mount
// => |
// => | --- | --- | --- mounted
// => |
// => | --- | --- | --- before update
// => |
// => | --- | --- | --- updated
// => |
// => | --- | --- before destroy
// => |
// => | --- destroy
```

我们可以看到我们的钩子按着步骤每一步来

当我们构建这个 vue 实例时候先触发 `beforeCreate`，在完成初始化后触发`created`。

在我们整个实例构建数据初始化完成之后，开始把 DOM 挂载触发 `beforeMount`，在完成了 DOM 的挂载之后触发 `mounted`。

当我们数据更新时候，发生改变之前会触发 `beforeUpdate` ，在数据更新完成后处罚 `updated`。

（这里要注意，我们会发现在这两个钩子中获取到的数据都是改变之后的数据。所以尽量少用两个钩子，至于这个是什么问题可以去关注一下 vue.js 的说明，因为这个问题已经被很多人说过了，但是还没相关的修复代码，而创始人说这其实并不是一个BUG。）

当我们去销毁该实例时候会在销毁之前触发 `beforeDestroy`，在销毁实例的最后一步触发 `destroyed`。

---

然后我们来说一下我们应该在什么钩子里做什么的样的事。

当我们通过这个例子去了解完了钩子触发的时机的时候，我们就能够在合理的钩子里做正常的事了。

### API请求数据

我们能够看到，当触发 `created` 钩子时，实例的数据已经完成了初始化了，我们可以在这个钩子里访问到我们在 `data` 里的数据了。

所以一般用于页面初始化的 API 数据请求，在这个钩子里就可以进行了。在完成请求后能设置到对应的 `data` 里面了。

### 事件监听

有关时间监听，我们一般能够在 `created` 和 `mounted` 这两个钩子里面进行监听。

在 `created` 钩子里面一般监听的是与 DOM 无关的事件，因为这个时候一些 DOM 可能还没有完成挂载。

在 `mounted` 钩子里能监听一些与 DOM 有关的时间，因为这个时候整个实例的 DOM 已经完成了挂载。

### 插件初始化

由于大部分我们引入的插件都与 DOM 有关，所以我比较建议在 `mounted` 钩子里面进行插件的初始化。

比如我们引入了 [echarts.js](http://echarts.baidu.com/) 这样的一个图标插件，如果我们在 `created` 后立刻初始化，可能会遇到的问题就是 DOM 没有完成挂载，则会出现报错（我试过了，当时还比较菜。）

### 取消一些时间监听或者清理一些全局的设置

我们一般在 `beforeDestroy` 钩子里，完成一些全局的比如 `计时器`、`时间监听`等的东西的取消监听。

比如我们熟悉的 `Event Bus` 如果不在事例结束前进行监听的 `$off` 的话，下次再进这个页面又回重新创建一个监听，这样子，我们钩子里面的代码就会多次触发。

## 结语

这一节介绍了 vue.js 的生命周期，我们需要了解这些钩子。方便我们去掌控整个实例，在什么时候做什么事，方便我们去实现我们需要实现的功能。

当然我们需要自己去理解整个过程，有时候我们还可以充分的利用这些钩子去一些黑科技。

时间: 2018-03-30

作者: Jeffery Liang

邮箱: lzh.jeffery@gmail.com

如果要转载请通过邮箱联系我。并声明原文地址。
