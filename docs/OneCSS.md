# CSS一句话笔记

1. padding设置百分比的话，百分比规定基于父元素的宽度的百分比的内边距。
> 使用场景：可以轻松写出不定宽度的正方形、长宽比明确的长方形
```
.square {
    width: 30%;
    padding-bottom: 30%;
} 
```

2. 元素设置`border-radius: 50%`后，设置单边的`border-color`加上`animation`就能实现一个简单的加载动画([优秀加载动画实现](https://tobiasahlin.com/spinkit/))
> 注：使用`transform`,`animation`需要注意布局，避免页面回流
```
.loading {
    width: 20px;
    height: 20px;
    border: 4px solid #f3f3f3;
    border-radius: 50%;
    border-top-color: #409eff;
    animation: myLoading 1s linear infinite;
}
@keyframes myLoading {
    0% {
        transform: rotate(0);
    }
    100% {
        transform: rotate(360deg);
    }
}
```

3. 设计稿有十字条纹或者井字条纹可以合理使用`:nth-child()`选取每行最后一个元素配合`::after`绘出上下隔开的分割线，再通过`:last-child::after`进行`display: none`隐藏最后一行的分割线
![十字条纹](https://s3.ax1x.com/2020/12/28/rTgMEq.png)
```

<style>
.box {
    position: relative;
    display: flex;
    flex-wrap: wrap;
}
.box::after {
    content: '';
    position: absolute;
    left: 50%;
    width: 1px;
    height: 100%;
    background: linear-gradient(0deg, rgba(255, 255, 255, 0) 0%, rgba(64, 230, 254, 0.97) 50%, rgba(255, 255, 255, 0) 100%);
    opacity: 0.4;
}
.box .cell {
    position: relative;
    width: 50%;
    height: 50px;
}
.box .cell:nth-child(2n)::after {
    content: '';
    position: absolute;
    bottom: 0;
    right: 0;
    width: 200%;
    height: 1px;
    background: linear-gradient(90deg, rgba(255, 255, 255, 0) 0%, rgba(64, 230, 254, 0.97) 50%, rgba(255, 255, 255, 0) 100%);
}
.box .cell:last-child::after {
    display: none;
}
</style>
<div class="box">
    <div class="cell">栅格</div>
    <div class="cell">栅格</div>
    <div class="cell">栅格</div>
    <div class="cell">栅格</div>
    <div class="cell">栅格</div>
</div>
```