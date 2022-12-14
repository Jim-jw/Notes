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

4. `transform`限制`position:fixed`的跟随效果，并会基于设置`transform`的元素定位

5. 属性`backdrop-filter`毛玻璃效果会影响`position:fixed`定位基点跟效果

6. 设置以下属性可以到达多行溢出省略号效果，但是仅限于webkit内核浏览器
```
<!-- 多行省略 -->
display: -webkit-box;
-webkit-box-orient: vertical;
-webkit-line-clamp: 3;
<!-- 一般的单行溢出省略 -->
white-space: nowrap;
overflow: hidden;
text-overflow: ellipsis;
```

7. CSS伪元素`::selection`将样式应用于用户突出显示的文字部分（例如，在文本上单击和鼠标拖动选中文本）
```
.highlighting::selection {
  background-color: red;
  color: #fff;
}
```

8. 计算属性值`calc()`
```
.main {
    width: calc(100% - 100px);
}
```

9. 使用`:empty`选择器来设置完全没有子元素或文本的元素的样式

10. `position: sticky`可以使用两行CSS创建粘性置顶效果，可以看看[这篇文章](https://www.zhangxinxu.com/wordpress/2018/12/css-position-sticky/)
```
.header {
    position: sticky;
    top: 0;
}
```

11. 使用`attr()`CSS函数创建CSS的动态文字提示框，`attr()`可以获取元素属性或自定义属性
```
// html
<span class="tooltip" data-tooltip="This is another Tooltip Content">here</span>
// css
.tooltip {
  position: relative;
  border-bottom: 1px dotted black;
}
.tooltip:after {
  content: "";
  position: absolute;
  bottom: 75%;
  left: 50%;
  margin-left: -5px;
  border-width: 5px;
  border-style: solid;
  opacity: 0;
  transition: opacity .6s;
  border-color: #062B45 transparent transparent transparent;
  visibility: hidden;
}

.tooltip:hover:before, .tooltip:hover:after {
  opacity: 1;
  visibility: visible;
}
```

12. 属性`caret-color`设置文本输入光标的颜色
```
input {
    caret-color: red;
}
```

13. `background-clip`设置花式文字
```
h1 {
    background: blue url('https://picsum.photos/id/1015/200/300');
    background-clip: text;
    -webkit-background-clip: text;
    color: transparent;
    margin-top: 20px;
    font-size: 120px;
}
```

14. `gap`设置`flex`布局下子元素行和列之间的间距
```
.flex {
    display: flex;
    gap: 10px;
}
```