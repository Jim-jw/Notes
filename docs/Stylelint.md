# 使用Sass的项目中集成[Stylelint](https://stylelint.io/)
> StyleLint 是『一个强大的、现代化的 CSS 检测工具』, 与 ESLint 类似, 是通过定义一系列的编码风格规则帮助我们避免在样式表中出现错误.

### 安装依赖
stylelint是运行工具，stylelint-config-standard是stylelint的推荐配置，stylelint-order是CSS属性排序插件,stylelint-scss引入了特定于SCSS语法的规则
```
npm install stylelint stylelint-config-standard stylelint-scss stylelint-order -D
OR
yarn add stylelint stylelint-config-standard stylelint-scss stylelint-order -D
```
文章当前几个依赖的版本：
```
"stylelint": "^13.13.1",
"stylelint-config-standard": "^22.0.0",
"stylelint-order": "^4.1.0",
"stylelint-scss": "^3.19.0",
```

### 配置 stylelint
在项目根目录建立`stylelint`配置文件 `.stylelintrc.js`(json格式也行，推荐使用js可以添加注释) ：
```
module.exports = {
  root: true,
  extends: ["stylelint-config-standard"],
  plugins: ["stylelint-order", "stylelint-scss"],
  ignoreFiles: ["src/assets/icon/*.css"],
  rules: {
    "at-rule-no-unknown": null,
    "scss/at-rule-no-unknown": true,
    "font-family-no-missing-generic-family-keyword": null, // 禁止在字体族名称列表中缺少通用字体族关键字。项目中做兼容，新项目可以注释掉
    "no-descending-specificity": null, // 禁止在具有较高优先级的选择器后出现被其覆盖的较低优先级的选择器。项目中做兼容，新项目可以注释掉
  }
}
```
添加忽略检查方法
- 第一种：在`stylelint`配置里面添加`"ignoreFiles": [文件地址]`
- 第二种：根目录添加`.stylelintignore`文件，直接添加需要忽略的文件夹或文件地址

### 使用
命令行：在 `package.json` 配置监测与修复命令：
```
"scripts": {
  "stylelint": "stylelint \"src/**/*.{css,scss,vue}\"",
  "stylefix": "stylelint \"src/**/*.{css,scss,vue}\" --fix"
}
```
不能自动修复部分，终端会报出wraning、error，需要根据提示信息手动修复

---
参考：[CSS代码检查工具stylelint的使用方法详解](https://www.jb51.net/article/179486.htm)、[stylelint/issues/1612](https://github.com/stylelint/stylelint/issues/1612)