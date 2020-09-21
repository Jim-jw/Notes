### 文件名`README.md`
> 一个项目的入门手册，里面介绍了项目描述、搭建方法、怎样进行本地开发等等

### 文件名`package.json`
> 列出项目所依赖的包。允许使用语义版本控制规则指定项目可以使用的包的版本。使构建可重现，因此更容易与其他开发人员共享。

### 文件名`vue.config.js`、`webpack.config.js`
> 整个项目的配置文件，输入输出路径、打包方式、包的处理等等

### 文件名`.gitignore`
> 项目管理使用[Git](https://github.com/github/gitignore)的话，配置`git`的忽略文件，在文件中申明哪些文件和文件夹不希望添加到git中去

常见配置
```
dist/
node_modules/
*.log
```

### 文件名`.editorconfig`
> [EditorConfig](https://editorconfig.org/)有助于维护跨多个编辑器和IDE从事同一项目的多个开发人员的一致编码风格。`VSCode`使用插件`EditorConfig for VS Code`解析

### 文件名`.eslintrc.js`、`.eslintrc.yaml`、`.eslintrc.yml`、`.eslintrc.json`
> [ESLint](http://eslint.cn/)是一个用来识别`ECMAScript`并且按照规则给出报告的代码检测工具，使用它可以避免低级错误和统一代码的风格。

### 文件名`.stylelint.js`、`.stylelint.yaml`、`.stylelint.yml`、`.stylelint.json`
> [StyleLint](https://stylelint.io/)是『一个强大的、现代化的 CSS 检测工具』, 与 ESLint 类似, 是通过定义一系列的编码风格规则帮助我们避免在样式表中出现错误。
