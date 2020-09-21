# [Webpack](https://www.webpackjs.com/concepts/mode/)入门
### 项目初始化
```
npm init
# OR
yarn init
```
---
### 引入webpack webpack-cli(webpack是核心模块，webpack-cli是命令行工具)
```
npm install webpack webpack-cli --save-dev
# OR
yarn add webpack webpack-cli --save-dev
```
> 因为我们此处是本地安装，所以无法在命令行使用`webpack`指令，需要使用`npx webpack`。
---
### 在根目录下添加`index.html`文件，添加`src`文件夹，在`src`添加`index.js`、`add-content.js`
<!-- - 在根目录添加scr文件夹作为源码目录，src属于默认目录，编辑后的资源输出在dist文件下，在执行build -->
index.html 添加基本标签后引入./dist/bundle.js
```
<script src="./dist/bundle.js"></script>
```
scr/add.js
```
export default function() {
  document.write("Hello World!!!")
}
```
src/index.js
```
import add from "./add-content"

document.write("First Webpack")
add()
```
### 项目根目录下命令行中执行`npx webpack --entry=./src/index.js --output-filename=bundle.js --mode=development`后用浏览器打开index.html查看效果
> 第一个参数`entry`是资源打包的入口，`webpack`中打包默认地址入口地址就是`./src/index.js`，所以打包命令中可以省略不写`npx webpack --output-filename=bundle.js --mode=development`

> 第二个参数`output-filename`是输出资源名，因为默认打包输出地址为项目根目录下的`./dist`文件夹下（没有会自动创建`dist`文件夹）

> 第三个参数`mode`指的是打包模式。Webpack提供了development、production、none三种模式。当置于development、production模式下，它会自动添加适合于当前模式的一系列配置，详细可见`Webpack`官网[模式(mode)](https://www.webpackjs.com/concepts/mode/)
---

### 使用`npm script`
> 上面操作可以看到每一次打包需要输入长命令，这样做不仅耗时而且容易出错。为了使命令行指令更加简洁，我们可以在`package.json`添加脚本命令  

`package.json`中`script`里面添加`"build": "webpack --output-filename=bundle.js --mode=development"`,如下：
```
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack --output-filename=bundle.js --mode=development"
}
```
终端命令行中执行`npm run build` or `yarn build`

---
### 使用配置文件
> 为了满足不同应用场景的需求`Webpack`拥有非常多的配置项以及相应的命令行参数。项目越来越多的配置时，需要往命令行中添加更多的参数，那么到了后期维护起来就会相当困难。为了解决这个问题，可以把这个参数改成对象放在默认的配置文件中，`Webpack`打包读取该配置文件就行了。`Webpack`的默认配置文件是`webpack.config.js`（也可以使用其他文件名，需要用命令行参数指定）

项目根目录下创建`webpack.config.js`，添加以下代码
```
module.exports = {
  // entry: '/src/index.js',
  output: {
    filename: 'bundle.js'
  },
  mode: 'development'
}
```
这时候`package.json`中`script`可以修改成
```
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack"
}
```
> 以上webpack的初始环境配置完毕
---
### 添加`webpack-dev-server`

项目安装`webpack-dev-server`
```
npm install webpack-dev-server --save-dev
# OR
yarn add webpack-dev-server --save-dev
```
为了方便启动`webpack-dev-server`，在`package.json`中添加一个dev指令：
```
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack",
    "dev": "webpack-dev-server --open"
}
```
需要对`webpack-dev-server`进行配置，编辑`webpack.config.js`如下：
```
module.exports = {
  // entry: '/src/index.js',
  output: {
    filename: 'bundle.js'
  },
  mode: 'development',
  devServer: {
    publicPath: '/dist'
  }
}
```
命令行中运行`npm run dev`，可以在终端输出log看到`Project is running at http://localhost:8080/`，浏览器打开`http://localhost:8080/`就可以看到`index.html`页面的内容，这时候去修改`./src/index.js`文件内容，页面会自动刷新
> 需要注意的是`webpack-dev-server`只是将打包结果放在内存中，并不会写入实际的`./dist/bundle.js`中，每次改动后都只是将内存中的打包结果返回给浏览器

---
**参考来源：**[Webpack实战：入门、进阶与调优](https://book.douban.com/subject/34430881/)、[Webpack官网](https://www.webpackjs.com/concepts/)