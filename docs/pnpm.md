# 包管理器后浪--`pnpm`
> Fast, disk space efficient package manager

### 一、 安装和使用
- 全局安装
```
npm i -g pnpm
```
- 使用方式跟使用`npm`和`npx`方法一致没有差别，分别用`pnpm`和`pnpx`代替

### 二、优势与长处
1. 依赖安装速度更快
    - 官方给出的对比数据

    | action  | cache | lockfile | node_modules| npm | pnpm | Yarn | Yarn PnP |
    | ---     | ---   | ---      | ---         | --- | --- | --- | --- |
    | install |       |          |             | 51s | 14.4s | 39.1s | 29.1s |
    | install | ✔     | ✔        | ✔           | 5.4s | 1.3s | 707ms | n/a |
    | install | ✔     | ✔        |             | 10.9s | 3.9s | 11s | 1.8s |
    | install | ✔     |          |             | 33.4s | 6.5s | 26.5s | 17.2s |
    | install |       | ✔        |             | 28.3s | 11.8s | 23.3s | 14.2s |
    | install | ✔     |          | ✔           | 4.6s | 1.7s | 22.1s | n/a |
    | install |       | ✔        | ✔           | 6.5s | 1.3s | 713ms | n/a |
    | install |       |          | ✔           | 6.1s | 5.4s | 41.1s | n/a |
    | update  | n/a   | n/a      | n/a         | 5.1s | 10.7s | 35.4s | 28.3s |
2. 不会重复安装同一个包，节省磁盘空间
3. 包的依赖管理更加优秀，下拉后的`node_modules`下的目录：`.pnpm`+`项目直接依赖项`，`.pnpm`里面目录平铺着所有依赖跟次级依赖，总结一下就是使用软链与平铺目录来构建一个嵌套结构。`yarn`和`npm`则是所有的依赖次级依赖都被平铺到`node_modules`目录下
4. 下拉依赖时候有更加简洁友好的输出提示
5. 支持`monorepo`
6. 安全性更高

*优秀文章指路*
- [关于现代包管理器的深度思考——为什么现在我更推荐 pnpm 而不是 npm/yarn?](https://juejin.cn/post/6932046455733485575#comment)