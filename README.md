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


## Question 4
你怎么理解vue中的diff算法？

### Answer: 


## Question 5
谈一谈对vue组件化的理解？

### Answer: 