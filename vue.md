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

## Question 3
你知道vue中key的作用和工作原理吗？说说你对它的理解。

### Answer: 

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
