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



### 6-监听属性 watch

watch 会实时监听数据变化并改变自身的值 

```html
<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8">
	<title>Vue 测试实例 - 菜鸟教程(runoob.com)</title>
	<script src="https://cdn.bootcss.com/vue/2.4.2/vue.min.js"></script>
</head>
   <body>
      <div id = "computed_props">
         千米 : <input type = "text" v-model = "kilometers">
         米 : <input type = "text" v-model = "meters">
      </div>
	   <p id="info"></p>
      <script type = "text/javascript">
         var vm = new Vue({
            el: '#computed_props',
            data: {
               kilometers : 0,
               meters:0
            },
            methods: {
            },
            computed :{
            },
            watch : {
               kilometers:function(val) {
                  this.kilometers = val;
                  this.meters = val * 1000;
               },
               meters : function (val) {
                  this.kilometers = val/ 1000;
                  this.meters = val;
               }
            }
         });
         // $watch 是一个实例方法
		vm.$watch('kilometers', function (newValue, oldValue) {
			// 这个回调将在 vm.kilometers 改变后调用
		    document.getElementById ("info").innerHTML = "修改前值为: " + oldValue + "，修改后值为: " + newValue;
		})
      </script>
   </body>
</html>
```



### 7-样式绑定 v-bind

`v-bind:class`

`v-bind:style`内联样式



### 8-事件处理器 v-on

```html
<div id="app">
  <button v-on:click="counter += 1">增加 1</button>
  <p>这个按钮被点击了 {{ counter }} 次。</p>
</div>
 
<script>
new Vue({
  el: '#app',
  data: {
    counter: 0
  }
})
</script>
```

```html
<!-- 同上 -->
<input v-on:keyup.enter="submit">
<!-- 缩写语法 -->
<input @keyup.enter="submit">
```

```html
<!-- 阻止单击事件冒泡 -->
<a v-on:click.stop="doThis"></a>
<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>
<!-- 修饰符可以串联  -->
<a v-on:click.stop.prevent="doThat"></a>
<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>
<!-- 添加事件侦听器时使用事件捕获模式 -->
<div v-on:click.capture="doThis">...</div>
<!-- 只当事件在该元素本身（而不是子元素）触发时触发回调 -->
<div v-on:click.self="doThat">...</div>

<!-- click 事件只能点击一次，2.1.4版本新增 -->
<a v-on:click.once="doThis"></a>
```



### 9-表单 v-model

输入框：input 和 textarea 元素中使用 v-model 实现双向数据绑定： 

```html
<div id="app">
  <p>input 元素：</p>
  <input v-model="message" placeholder="编辑我……">
  <p>消息是: {{ message }}</p>
    
  <p>textarea 元素：</p>
  <p style="white-space: pre">{{ message2 }}</p>
  <textarea v-model="message2" placeholder="多行文本输入……"></textarea>
</div>
 
<script>
new Vue({
  el: '#app',
  data: {
    message: 'Runoob',
    message2: '菜鸟教程\r\nhttp://www.runoob.com'
  }
})
</script>
```

单选框radio、复选框checkbox、下拉列表select

修饰符：.lazy .number .trim