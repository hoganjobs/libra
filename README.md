# libra

## Question 1
v-if和v-for哪个优先级高？如果两个同时出现，应该怎么优化得到更好的性能？

### Answer
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

