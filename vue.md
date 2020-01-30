# Vue

## Question 1
v-if和v-for哪个优先级高？如果两个同时出现，应该怎么优化得到更好的性能？

### Answer: 
源码compiler/codegen/index.js

1、显然v-for优先于v-ifv-if别解析（在源码中index.js的64行）
```
  if (el.staticRoot && !el.staticProcessed) {
    return genStatic(el, state)
  } else if (el.once && !el.onceProcessed) {
    return genOnce(el, state)
  } else if (el.for && !el.forProcessed) {
    return genFor(el, state)
  } else if (el.if && !el.ifProcessed) {
    return genIf(el, state)
  } else if (el.tag === 'template' && !el.slotTarget && !state.pre) {
    return genChildren(el, state) || 'void 0'
  } else if ...
```

2、如果同时出现，每次渲染都会先执行循环再判断条件，无论如何循环都不可避免，浪费了性能

3、要避免出现这种情况，则在外层嵌套template，在这一层进行v-ifv-if判断，然后在内部进行v-for循环

## Question 2 
Vue组件data选项为什么必须是个函数而Vue的根实例则没有此限制？

### Answer: 
源码src\core\instance\state.js - initData()

1、Vue组件可能存在多个实例，如果使用对象形式定义data，则会导致它们共用一个data对象，那么状态变更将影响所有组件实例，这是不合理的；

2、采用函数形式定义，在initData时会将其作为工厂函数返回到全新data对象，有效规避多实例之间状态污染问题。

3、而在Vue根实例创建过程中则不存在该限制，也是因为根实例只能有一个，不需要担心这种情况。

```
  data = vm._data = typeof data === 'function'
    ? getData(data, vm)
    : data || {}
```

## Question 3
你知道vue中key的作用和工作原理吗？说说你对它的理解。

### Answer: 
源码src\core\vdom\patch.js - updateChildren()

1、key的作用主要是为了高效的更新虚拟DOM，其原理是vue在patch过程中通过key可以精准判断两个节点是否是同一个，从而避免频繁更新不同元素，使得整个patch过程更加高效，减少DOM操作量，提高性能。

2、另外，若不设置key还可能在列表更新时引发一些隐蔽的bug

3、vue中在使用相同标签名元素的过渡切换时，也会使用到key属性，其目的也是为了让vue可以区分它们，否则vue只会替换其内部属性而不会触发过渡效果。

```
bredkpoint:
oldStartVnode.tag==='p'

else if (sameVnode(oldStartVnode, newStartVnode)) {
    patchVnode(oldStartVnode, newStartVnode, insertedVnodeQueue, newCh, newStartIdx)
    oldStartVnode = oldCh[++oldStartIdx]
    newStartVnode = newCh[++newStartIdx]
}

```

## Question 4
你怎么理解vue中的diff算法？

### Answer: 
源码分析1: 必要性，lifecyle.js - mountComponent()

组件中可能存在很多个data中的key使用

源码分析2: 执行方式，patch.js - patchVnode()

patchVnode是diff算法发生的地方，整体策略：深度优先，同层比较

源码分析3: 高效性，patch.js - updateChildren()

3W1H原则答

1、diff算法是虚拟DOM技术的必然产物：通过新旧虚拟DOM作对比（即diff），将变化的地方更新在真实DOM上；另外，也需要diff高效的执行对比过程，从而降低时间复杂度为O(n)。

2、vue 2.x 中为了降低 Watcher 粒度，每个组件只有一个Watcher与之对应，只有引入diff才能精确找到发生变化的地方。

3、vue中diff执行的时刻是组件实例执行其更新函数时，它会对比上一次渲染结果oldVnode和新的渲染结果newVnode，此过程称为patch。

4、diff过程整体遵循深度优先、同层比较的策略；两个节点之间比较会根据它们是否拥有子节点或者文本节点做不同操作；比较两组子节点是算法的重点，首先假设头尾节点可能相同做4次尝试，如果没有找到相同节点才按照通用方式遍历查找，查找结束再按情况处理剩下的节点；借助key通常可以非常精确找到相同节点，因此整个patch过程非常高效。

## Question 5
谈一谈对vue组件化的理解？

### Answer: 
回答思路:

组件化定义、优点、使用场景和注意事项等方面展开陈述，同时要强调vue中组件化的一些特点；

组件化，把独立功能的一些模块切分出来；

源码分析1: 组件定义，src\core\global-api\assets.js

vue-loader会编译template为render函数，最终导出的依然是组件配置对象。

源码分析2: 组件化优点

lifecyle.js - mountComponent()

组件、Watcher、渲染函数和更新函数之间的关系

源码分析3: 组件化实现

构造函数，src\core\global-api\extend.js

实例化及挂载，src\core\vdom\patch.js - createElm()

Summary:

1、组件是独立和可复用的代码组织单元。组件系统是Vue核心特性之一，它使开发者使用小型、独立和通常可复用的组件构建大型应用；

2、组件化开发能力大幅提高应用开发效率、测试性、复用性等

3、组件使用按分类有： 页面组件、业务组件、通用组件；

4、vue的组件是基于配置的，我们通常编写的组件是组件配置而非组件，框架后续会生成其构造函数，它们基于VueComponent，扩展于Vue;

5、vue中常见组件化技术有: 属性prop，自定义事件，插槽等，它们主要用于组件通信、扩展等；

6、合理的划分组件，有助于提升应用性能；

7、组件应该是高内聚、低耦合的；

8、遵循单向数据流的原则。

## Question 6
谈一谈对vue的设计原则的理解？

### Answer:

## Question 7
vue为什么要求组件模板只能有一个根元素？

### Answer:
从三方面考虑;

1、new Vue({el:'#app'})

2、单文件组件中，template下的元素div。其实就是“树”状数据结构中的“根”。

3、diff算法要求的，源码中，patch.js里patchVnode()。

如果同时设置了多个入口，那么vue就不知道哪一个才是这个“类”

在webpack搭建的vue开发环境下，使用单文件组件时：
```
<template>
    <div></div>
</template>
```
template这个标签，它有三个特性：
```
1、隐藏性： 该标签不会显示在页面的任何地方，即使里面有多少内容，它永远都是隐藏的状态，设置了display:none;
2、任意性：该标签可以写在任何地方，甚至是head、body、script标签内；
3、无效性：该标签里的任何HTML内容都是无效的，不会起任何作用；只能innerHTML来获取里面的内容。
```

一个vue单文件组件就是一个vue实例，如果template下有多个div那么如何指定vue实例的根入口呢，为了让组件可以正常生成一个vue实例，这个div会自然的处理成程序的入口，通过这个根节点，来递归遍历整个vue树下的所有节点，并处理为vdom，最后在渲染成真正的HTML，插入在正确的位置。

## Question 8
谈谈你对MVC、MVP和MVVM的理解？

### Answer:

## Question 9
谈谈你对vue组件之间通信的理解？

### Answer:

## Question 10
你了解哪些vue性能优化方法？

### Answer:
