## Vue CLI快速原型开发
> 可以使用 `vue serve` 和 `vue build` 命令对单个 `*.vue` 文件进行快速原型开发，需要安装全局安装 `@vue/cli` 和 `@vue/cli-service-global`

### Vue CLI的安装
> #### 关于旧版本
> Vue CLI 的包名称由 `vue-cli` 改成了 `@vue/cli`。 如果你已经全局安装了旧版本的 `vue-cli` (1.x 或 2.x)，你需要先通过 `npm uninstall vue-cli -g` 或 `yarn global remove vue-cli` 卸载它。

安装命令
```
npm install -g @vue/cli
# OR
yarn global add @vue/cli
```
安装之后，你就可以在命令行中访问 `vue` 命令。你可以通过简单运行 `vue`，看看是否展示出了一份所有可用命令的帮助信息，来验证它是否安装成功。也可以用 `vue --version` 来检查其版本是否正确。

升级

如需升级全局的 Vue CLI 包，请运行：
```
npm update -g @vue/cli
# OR
yarn global upgrade --latest @vue/cli
```

### `@vue/cli-service-global`依赖的安装
```
npm install -g @vue/cli-service-global
# OR
yarn global add @vue/cli-service-global
```
### 本地快速原型开发
- 文件夹内新建`App.vue`文件：
```
<template>
  <h1>Hello!</h1>
</template>
```
- 然后在这个`App.vue`文件所在的目录下运行：
```
vue serve -o
```
> `vue serve`会在当前目录自动推导入口文件——入口可以是 main.js、index.js、App.vue 或 app.vue 中的一个。你也可以显式地指定入口文件：
```
vue serve MyComponent.vue
```

---
> 扩展：`Vue CLI`快速原型开发怎么引入`Element UI`
>> 解决思路[参考](https://juejin.im/post/6844904080402284552),实现的仓库[地址](https://github.com/Jim-jw/vue-cli-service-element-ui)