# Vue.js

### 0-

参考：http://www.runoob.com/vue2/vue-tutorial.html

### 1-安装

node.js->npm->cnpm->Vue

参考：http://www.runoob.com/vue2/vue-install.html

```
$ cd my-project
$ cnpm install
$ cnpm run dev
 DONE  Compiled successfully in 4388ms

> Listening at http://localhost:8080
```

### 2-一个简单的Vue应用

#####  · 实例化一个vue

​	 `var vm  = new Vue({   })`

#####  · el参数

​	对应DOM元素中的id

#####  · data参数

​	用于定义属性

#####  · methods参数

​	用于定义的导函数，可以通过return来返回函数值。

#####  · {{  }}

​	用于输出对象属性和函数返回值。

#####  · $

​	其他实例属性与方法

### 3-条件语句 v-if

```html
<div id="app">
    <p v-if="seen">现在你看到我了</p>
    <template v-if="ok">
      <h1>菜鸟教程</h1>
    </template>
</div>
    
<script>
new Vue({
  el: '#app',
  data: {
    seen: true,
    ok: true
  }
})
</script>
```

还有以下几种用法：

`v-else`

`v-else-if`

`v-show`

##### Q: **v-if 与 v-show 的区别？**

1.

在切换 v-if 块时，Vue.js 有一个局部编译/卸载过程，因为 v-if 之中的模板也可能包括数据绑定或子组件。v-if 是真实的条件渲染，因为它会确保条件块在切换当中合适地销毁与重建条件块内的事件监听器和子组件。

v-if 也是惰性的：如果在初始渲染时条件为假，则什么也不做——在条件第一次变为真时才开始局部编译（编译会被缓存起来）。

相比之下，v-show 简单得多——元素始终被编译并保留，只是简单地基于 CSS 切换。

一般来说，v-if 有更高的切换消耗而 v-show 有更高的初始渲染消耗。因此，如果需要频繁切换 v-show 较好，如果在运行时条件不大可能改变 v-if 较好。

2.

v-if 是动态添加，当值为 **false** 时，是完全移除该元素，即 **dom** 树中不存在该元素。

v-show 仅是隐藏 / 显示，值为 **false** 时，该元素依旧存在于 **dom** 树中。若其原有样式设置了 **display: none** 则会导致其无法正常显示。

### 4-循环语句 v-for

```html
<div id="app">
  <ol>
    <li v-for="site in sites">
      {{ site.name }}
    </li>
  </ol>
</div>
 
<script>
new Vue({
  el: '#app',
  data: {
    sites: [
      { name: 'Runoob' },
      { name: 'Google' },
      { name: 'Taobao' }
    ]
  }
})
</script>
```

可以迭代对象，迭代整数等。

迭代对象可以加三个参数`index` `key` `value`

```html
<div id="app">
  <ul>
    <li v-for="(value, key, index) in object">
     {{ index }}. {{ key }} : {{ value }}
    </li>
  </ul>
</div>
```

### 5-计算属性 computed

`getter` & `setter`

我们可以使用 methods 来替代 computed，效果上两个都是一样的，但是 computed 是基于它的依赖缓存，只有相关依赖发生改变时才会重新取值。而使用 methods ，在重新渲染的时候，函数总会重新调用执行。 