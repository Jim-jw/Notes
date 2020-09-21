# 软件包管理器以及Yarn与NPM包管理器

## Windows下的软件包管理器
> 自动配置path
- Windows10的Winget以及[简单体验](https://zhuanlan.zhihu.com/p/143123689)
- [Chocolatey](https://chocolatey.org/)
- [Scoop](https://sspai.com/post/52496)

### Chocolatey的安装
- 进去Windows的命令行工具`cmd` or `Windows PowerShell`(注：管理员的身份去运行命令行工具)
- 复制以下内容，回车执行等待就行（复制内容来自[官网](https://chocolatey.org/install)）
```
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```
- 安装完毕之后，输入`choco`会出现Chocolatey版本号就说明安装成功了

### Chocolatey的常用使用命令
```
choco install git node yarn firefox # 下载软件
choco install --yes git node firefox  # 默认选项下载软件
choco list -local # 查看本地安装的软件(通过choco install安装)
choco uninstall git # 卸载软件
choco outdated # 查看需要更新的软件
choco upgrade --yes git # 升级指定软件
choco upgrade --yes all # 升级全部程序
choco upgrade chocolatey  # 更新chocolatey自身
choco info git # 查看程序的详细信息
```
## macOS和Linux下的软件包管理器Homebrew
- [官方安装方式](https://brew.sh/index_zh-cn)会比较慢，需要自行查找国内镜像安装
### Homebrew的常用使用命令
```
brew –help # 查看brew的帮助
brew install git # 下载软件
brew uninstall git # 卸载软件
brew list # 查看本地安装的软件
brew upgrade git # 升级指定软件
brew upgrade # 升级全部程序
brew update # 更新Homebrew
```
## Yarn与NPM包管理器
- 安装Yarn和NPM(NPM安装node会自带)
- Yarn和NPM常用命令

|作用|Yarn|NPM|
|:-|:-:|-:|
|项目初始化|yarn init|npm init|
|下载项目依赖|yarn|npm install|
|添加依赖包|yarn add vue|npm install vue|
|全局依赖包|yarn global add vue|npm install vue -g|
|删除依赖包|yarn remove vue|npm uninstall vue|
|package.json中scripts定义的命令|yarn dev/yarn run dev|npm run dev|
|||
- Yarn的下载源也可以设置为淘宝镜像
```
$ yarn config get registry // 查看当前下载源,初始为https://registry.yarnpkg.com
$ yarn config set registry https://registry.npm.taobao.org // 更改为淘宝
```
---
### Yarn的优点
1. 并行安装
> npm 是按照队列执行每个 package，也就是说必须要等到当前 package 成功安装之后，才能继续后面的安装。而 Yarn 是同步执行所有任务，提高了性能。
2. 执行校验
> 在执行代码之前，Yarn 会通过算法校验每个安装包的完整性。
3. 离线模式
> 离线的原理比较简单，安装过的包会被保存进缓存目录，以后安装就直接从缓存中复制过来，这样做的本质还是会提高安装下载的速度，避免不必要的网络请求。


