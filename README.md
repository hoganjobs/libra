# libra

## Question 1
v-if和v-for哪个优先级高？如果两个同时出现，应该怎么优化得到更好的性能？

### Answer: 
1、v-for比v-if具有更高的优先;

2、v-for会先被执行生成虚拟节点，后再执行v-if判断渲染与否，例如经过如下运算：
```
this.list.map(function (item) {
  if (item.isActive) {
    return item.content
  }
})
```

3、在使用上存在两种情况：
- 为了过滤一个列表中的项目
- 为了避免渲染本应该被隐藏的列表

4、针对以上使用存在两种情况：
- 使用计算属性过滤出要渲染的列表的项目，生成一个新列表数组；
- 使用外层包裹元素进行v-if判断整个列表渲染；

## Question 2 
Vue组件data选项为什么必须是个函数而Vue的根实例则没有此限制？

### Answer: 
1、首先，Javascipt里Object是引用数据类型，如果不用function 返回，每个组件的data 都是内存的同一个地址，一个数据改变了其他也改变了;

2、其二，在Vue中，一个组件的 data 选项必须是一个函数，以保持每个实例可以维护一份被返回对象的独立的拷贝，因为组件可能被用来创建多个实例;

3、假设 data 是一个纯粹的对象，则所有的实例将共享引用同一个数据对象；

4、data作为函数在每次创建一个新实例后，能够调用 data 函数，从而返回初始数据的一个全新副本数据对象；

5、最后，使用Vue的根实例时，本身就只有一个实例，是定义在实例中的data属性，所以在Vue的根实例上可以直接使用对象的方式；
```
new Vue({
  el: '#app',
  data: {

  }
})
```

6、补充，Vue.extend() 中 data 也必须是函数；

## Question 3
你知道vue中key的作用和工作原理吗？说说你对它的理解。

### Answer: 
1、作用：
    key的作用主要是为了高效的更新虚拟DOM

2、工作原理：
    在渲染真实dom之前都会生成虚拟dom，当某个节点数据改变时，对比新旧虚拟dom的节点；
    按照深度优先，同层比较的原则，在patch 函数中执行，判断两个节点是否相同可直接，如果设置了key，就会用key进行比较。

## Question 4
你怎么理解vue中的diff算法？

### Answer: 
1、首先，什么是diff算法。在vue数据响应中每一次数据更新要直接操作真实dom开销很大，为了解决和优化这个问题，会先生成虚拟dom，每次数据改变对比新旧虚拟dom再根据对比结果操作真实dom，而diff算法就是对比新旧两个虚拟dom是否相等；

2、使用diff算法可以优化性能减少开销，跨平台，兼容性等优势；

3、diff算法在vue源码中的patch函数实现，对比新旧虚拟dom的vnode节点；

4、diff算法的比较原则是深度优先，同级比较。

## Question 5
谈一谈对vue组件化的理解？

### Answer: 
1、它提供一种抽象，使之可以使用独立可复用的组件来构建大型应用；

2、组件化是vue的核心思想，就是把页面拆分成多个组件，每个组件资源独立、可复用，并且组件与组件之间可以嵌套；

3、组件也分为页面组件、基础组件以及功能组件；

4、组件包含prop属性、自定义事件、slot扩展插槽等三要素；

5、一个组件的生成主要为：
    创建组件构造函数，继承于Vue
    挂载组件钩子函数，在patch流程中触发
    实例化组件vnode

6、组件化优势，在于能提高开发效率，方便重复使用，简化调试步骤，提升项目可维护 性，便于多人协同开发。

## Question 6
谈一谈对vue的设计原则的理解？

### Answer:
1、用组件化的思想构建应用；

2、合理细分颗粒化；

3、场景化，按照不同状态封装组件；


## Question 7
vue为什么要求组件模板只能有一个根元素？

### Answer:
1、组件的template最终会转换成VNode对象，一个组件对应一个根元素对应一个VNode对象

2、从效率上，如果多个根，那么就会产生多个入口（遍历、查找）从效率上来说都不方便

3、同时也取决于diff算法，单个匹配，深度优先，同级比较；

## Question 8
谈谈你对MVC、MVP和MVVM的理解？

### Answer:
1、Model-View-Controller，所有通信都是单向的
```
模型（Model）：数据库相关的操作、文件的访问和数据结构等。

视图（View）：专注于显示，用户界面，如Web前端（HTML/CSS/Java Script）

控制器（Controller）：业务逻辑，连接模型和视图，如把视图的请求发送给模型或把数据返回给视图等
```

2、Model-View-Presenter，改变了通信方向，通信都是双向的；View 与 Model 不发生联系，都通过 Presenter 传递。View 不部署任何业务逻辑，称为"被动视图"（Passive View）
```
模型（Model）：数据库相关的操作、文件的访问和数据结构等。

视图（View）：专注于显示，如Web前端（HTML/CSS/Java Script）

展示器（Presenter）：连接模型和视图，处理视图的请求并根据模型更新视图
```

3、Model-View-ViewModel，双向绑定
```
模型（Model）：数据库相关的操作、文件的访问和数据结构等。

视图（View）：专注于显示，用户界面，如Web前端（HTML/CSS/Java Script）

视图模型（ViewModel）：连接模型和视图，视图模型和视图是双休绑定的。
```

## Question 9
谈谈你对vue组件之间通信的理解？

### Answer:
1、先盘点一下vue组件之间的通信：
```
props
$emit / $on
($parent / $children) / $refs
vuex
bus
provide / inject
$attrs / $listeners
```

2、常用的方式有props、vuex、事件总线bus；

3、其他一些边界情况$parent、$children、 $refs、provide / inject；

4、还有非props特性的$attrs、$listeners；

5、常见的props，父组件通过props将数据下发给props，子组件通过$emit来触发自定义事件来通知父组件进行相应的操作；
```
// 子组件
props: { msg: String }

// 父组件
<Children :msg="msg" v-on:changeMsg="changeMsg"/>
```

6、$parent/$children 都是通过获取父组件实例或子组件实例；
```
// 子组件
this.$parent.changeMsg();

// 父组件
this.$children[0].msg;
```

7、$refs,给元素或子组件注册引用信息,引用信息将会注册在父组件的 $refs 对象上；
```
// 父组件
this.$refs.child.msg;
```

8、provide、inject，能够实现祖先和后代之间传值，允许一个祖先组件向其所有子孙后代注入一个依赖,不论组件层次有多深,并在起上下文关系成立的时间里始终生效
```
// 孙/子组件
inject: ['msg']

// 父组件
provide() {
    return {
        msg: this.msg
    }
}
```

9、$attrs，当一个组件没有声明任何 prop 时(没有在props声明属性)，这里会包含所有父作用域的绑定 ，并且可以通过 v-bind="$attrs" 传入内部组件；
```
// 孙组件
{{$attrs.msg}}
```

10、$listeners，包含了父作用域中的 (不含 .native 修饰器的) v-on 事件监听器。它可以通过 v-on="$listeners" 传入内部组件；
```
// 子组件
<Children v-bind="$attrs" v-on="$listeners"/>
```

## Question 10
你了解哪些vue性能优化方法？

### Answer:
1、v-for 遍历避免同时使用 v-if，在外层包裹进行判断或通过computed过滤出新的渲染列表；
```
<template v-if="isFodlder">
  <p v-for="item in list">{{item.title}}</p>
</template>

this.list.map(function (item) {
  if (item.isActive) {
    return item.content
  }
})
```

2、事件的销毁，使用 addEventListene 等方式添加的事件要手动移除
```
created() {
  addEventlistener('click', this.click, false)
},
beforeDestroy() {
  removeEventListener('click', this.click, false);
}
```

3、图片资源懒加载，使用vue-lazyload 等插件对图片资源进行懒加载处理，优化页面加载性能；

4、路由懒加载；
```
const Foo = () => import('./Foo.vue');

const routes = [
  {
    path: '/foo',
    component: Foo
  }
]

const router = new VueRouter({
  routes
})
```