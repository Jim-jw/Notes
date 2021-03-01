# `ElementUI`之那些你不知道的事

1. **内置了滚动条组件`el-scrollbar`**
    ##### 简单使用方法
    ```
    // 需要设置el-scrollbar父元素标签的固定高度(不能使用calc)和overflow: hidden
    // 需要设置scrollbar自身的height: 100%
    <div style="height: 100px;overflow: hidden;">
        <el-scrollbar style="height: 100%">
        <!-- content -->
        </el-scrollbar>
    </div>
    <!-- style -->
    // 一般不需要x轴的滚动条所以需要手动设置scrollbar的overflow-x: hidden，这里不要设置全局样式，会引起Select组件的下拉高度不准确的bug
    /deep/.el-scrollbar__wrap {
        overflow-x: hidden;
    }
    ```

2. **上传组件`Upload`的批量上传其实是多次调用接口达到批量上传** 
    ##### 解决批量上传只走一次接口（例子给的是手动上传的解决方法）
    - 设置`el-upload`的`ref`值如：`upload`，关闭自动上传`:auto-upload="false"`
    - 点击上传按钮通过`this.$refs.upload.uploadFiles`获取待上传的文件然后进行数据处理，关键代码如下：
    ```
    const fileList = this.$refs.upload.uploadFiles
    const formDatas = new FormData()
    fileList.forEach(item => {
        formDatas.append('files', item.raw)
    })
    <!-- 这里设置了全局的Vue.prototype.axios，所以这样写 -->
    this.axios({
        method: 'POST',
        url: '', // 接口路径
        headers: {
            'Content-Type': 'multipart/form-data',
        },
        data: formDatas,
    })
    ```
    - 上传后记得使用`this.$refs.upload.clearFiles()`清理掉待上传的文件，一是避免第二次上传上传了之前的文件数据，二是可以垃圾回收释放内存


3. **选择器组件`Select`等类型组件的下拉弹出的层级`z-index`值和弹窗的遮罩层的`z-index`值用的是同一套逻辑从`2000`开始每点击一次递增，会导致在弹窗里面使用`el-select`会发现下拉框可能不出现的bug**

4. **滑块组件`Slider`设置`width: 100%`，拖动的小圆球按钮会导致父元素出现横向滚动条**

5. **日期选择器组件`DatePicker`源码中有做`showWeekNumber`显示周数的功能，但是不知道为什么没有进行下去，想实现显示周数功能的可以参考[这篇文章](https://www.cnblogs.com/mianbaodaxia/p/11170251.html)**

6. **分页组件`Pagination`可以通过传`align`值`'left' | 'center' | 'right'`设置页码的居左、中、右对齐方式**

> 注：文章是对使用`ElementUI`一些文档之外小技巧的一个记录,非常感谢`ElementUI`团队以及提出自己`pr`做出贡献的人,文章写的还有不懂的地方或者补充的地方欢迎讨论跟补充