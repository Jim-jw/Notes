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

6. `HTTP`消息头中`Cache-Control`可用值`no-store`跟`no-cache`
    - `no-store`: **「永远都不要在客户端存储资源」**，每次永远都要从原始服务器获取资源
    - `no-cache`: 可以在客户端存储资源，但每次都 **「必须去服务器做新鲜度校验」**，来决定从服务器获取最新资源 (200) 还是从客户端读取缓存 (304)，即所谓的协商缓存
7. `HTTP`消息头中的`httponly`属性：若此属性为true，则只有在http请求头中会带有此cookie的信息，而不能通过document.cookie来访问此cookie

8. `COMMONJS`和`ES6`模块区别
    1. CommonJS是对模块的浅拷⻉，ES6 Module是对模块的引⽤，即ES6 Module只存只读，不能改变其值，也就是指针指向不能变，类似const
    2. import的接⼝是read-only（只读状态），不能修改其变量值。 即不能修改其变量的指针指向，但可以改变变量内部指针指向，可以对commonJS对重新赋值（改变指针指向），但是对ES6 Module赋值会编译报错

9. 违反浏览器同源策略就叫跨域, 同源策略是协议和域名以及端口必须一致

10. `Webpack`的`Loader`和`Plugin`
    - `Loader`直译为"加载器"。`Webpack`将⼀切⽂件视为模块，但是`Webpack`原⽣是只能解析`JavaScript`和`JSON`⽂件，如果想将其他⽂件也打包的话，就会⽤到`Loader`。 所以`Loader`的作⽤是让`Webpack`拥有了加载和解析非`JavaScript`文件的能力;
    - `Plugin`直译为"插件"。`Plugin`可以扩展`Webpack`的功能,让`Webpack`具有灵活性。在`Webpack`运⾏的⽣命周期中会⼴播出许多事件，`Plugin`可以监听这些事件，在合适的时机通过`Webpack`提供的`API`改变输出结果。

11. `WebPack`中`bundle`,`chunk`,`module`
    - `bundle`是由Webpack打包出来的文件;
    - `chunk`代码块,一个`chunk`由多个模块组合⽽成，⽤于代码的合并和分割;
    - `module`是开发中的单个模块，在`Webpack`中⼀个模块对应⼀个⽂件，`Webpack`会从配置的`entry`中递归开始找出所有依赖的模块。
