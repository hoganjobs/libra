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

## Question 3
你知道vue中key的作用和工作原理吗？说说你对它的理解。

### Answer: 
源码src\core\vdom\patch.js - updateChildren()

1、key的作用主要是为了高效的更新虚拟DOM，其原理是vue在patch过程中通过key可以精准判断两个节点是否是同一个，从而避免频繁更新不同元素，使得整个patch过程更加高效，减少DOM操作量，提高性能。

2、另外，若不设置key还可能在列表更新时引发一些隐蔽的bug

3、vue中在使用相同标签名元素的过渡切换时，也会使用到key属性，其目的也是为了让vue可以区分它们，否则vue只会替换其内部属性而不会触发过渡效果。

## Question 4
你怎么理解vue中的diff算法？

### Answer: 

## Question 5
谈一谈对vue组件化的理解？

### Answer: 

## Question 6
谈一谈对vue的设计原则的理解？

### Answer:

## Question 7
vue为什么要求组件模板只能有一个根元素？

### Answer:

## Question 8
谈谈你对MVC、MVP和MVVM的理解？

### Answer:

## Question 9
谈谈你对vue组件之间通信的理解？

### Answer:

## Question 10
你了解哪些vue性能优化方法？

### Answer:
