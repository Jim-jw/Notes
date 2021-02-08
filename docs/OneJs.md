# Js一句话笔记

1. JS引擎是单线程的，但是浏览器是多线程的。现代浏览器的一个tab，其中的线程包括但不局限于
    - GUI 渲染线程
    - JS引擎线程
    - 事件触发线程
    - 定时器触发线程
    - 异步http请求线程

2. GUI 渲染和JavaScript执行是互斥的。虽然两者属于不同的线程，但是由于JavaScript执行结果可能会对页面产生影响，所以浏览器对此做了处理，大部分情况下JavaScript线程执行，执行渲染（render）的线程就会暂停，等JavaScript的同步代码执行完再去渲染。

3. 使用`Array.isArray()`判断是否为数组

4. 使用`arr.length = 0`来重置数组数据

5. `Set`是一种叫做集合的数据结构，`Map`是一种叫做字典的数据结构----[来源](https://vue3js.cn/es6/dataStructure.html#%E8%83%8C%E6%99%AF)
    - 集合，是由一堆无序的、相关联的，且不重复的内存结构【数学中称为元素】组成的组合
    - 字典（dictionary）是一些元素的集合。每个元素有一个称作key 的域，不同元素的key 各不相同
    - 共同点：集合、字典都可以存储不重复的值
    - 不同点：集合是以[值，值]的形式存储元素，字典是以[键，值]的形式存储
```
    // Set简单使用场景
    // 数组去重
    let arr = [1, 1, 2, 3];
    let unique = [... new Set(arr)];

    let a = new Set([1, 2, 3]);
    let b = new Set([4, 3, 2]);
        
    // 并集
    let union = [...new Set([...a, ...b])]; // [1,2,3,4]
        
    // 交集
    let intersect = [...new Set([...a].filter(x => b.has(x)))]; // [2,3]
        
    // 差集
    let difference = Array.from(new Set([...a].filter(x => !b.has(x)))); // [1]
```