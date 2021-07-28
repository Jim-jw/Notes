# `Git`提交说明规范

## 规范介绍

- `git commit`信息格式介绍
```
type(scope): subject
```
1. type(必须): commit 的类别，只允许使用下面几个标识：
```
feat:     新的功能,
fix:      修复Bug,
docs:     只有文档变更,
style:    空格, 分号等格式修复,
refactor: 代码重构，注意和特性、修复区分开,
perf:     提升性能,
test:     添加测试,
chore:    开发工具变动(构建、脚手架工具等),
```
2. scope(可选) : 用于说明改动的影响范围
3. subject(必须) : commit 的简短描述，不超过50个字符

## 实际应用

1. `git commit -m`提交规范示例
```
git commit -m"feat: test"
#OR
git commit -m"feat(test): test"
```

2. 使用Cz工具集(推荐)

    1. 安装cz工具
    ```
    npm install -g commitizen
    ```
    2. 使用`git cz`代替`git commit -m`
    > `git cz -a`等于`git commit -am`

## 项目中集成规范
1. 安装依赖
```
npm install --save-dev @commitlint/cli @commitlint/config-conventional cz-customizable husky@4.3.8
```
2. 在`package.json`中新增`config.commitizen`字段信息、配置`git commit`提交时的校验钩子
```
"config": {
    "commitizen": {
        "path": "./node_modules/cz-customizable"
    }
},
"husky": {
    "hooks": {
        "commit-msg": "commitlint -E HUSKY_GIT_PARAMS"
    }
}
```
3. 根目录下新建`commitlint.config.js`文件，复制下面内容设置校验规则
```
module.exports = {
    extends: ['@commitlint/config-conventional']
};
```
4. 根目录下新建`.cz-config.js`文件，复制下面定制提交说明
```
'use strict';

module.exports = {

  types: [
    {value: 'feat',     name: 'feat:     新的功能'},
    {value: 'fix',      name: 'fix:      修复Bug'},
    {value: 'docs',     name: 'docs:     只有文档变更'},
    {value: 'style',    name: 'style:    空格, 分号等格式修复'},
    {value: 'refactor', name: 'refactor: 代码重构，注意和特性、修复区分开'},
    {value: 'perf',     name: 'perf:     提升性能'},
    {value: 'test',     name: 'test:     添加测试'},
    {value: 'chore',    name: 'chore:    开发工具变动(构建、脚手架工具等)'},
    {value: 'revert',   name: 'revert:   代码回退'},
  ],

//   scopes: [],

  // override the messages, defaults are as follows
  messages: {
    type: '选择一种你的提交类型:',
    // scope: '\nDenote the SCOPE of this change (optional):',
    // used if allowCustomScopes is true
    customScope: '表示此更改的范围(可选):',
    subject: '短说明(必填):\n',
    body: '长说明，使用"|"换行(可选):\n',
    breaking: '非兼容性说明 (可选):\n',
    footer: '关联关闭的issue，例如：#31, #34(可选):\n',
    confirmCommit: '确定提交commit记录?'
  },

  allowCustomScopes: false,
  allowBreakingChanges: ['feat', 'fix'],

  // limit subject length
  subjectLimit: 100

};
```

---
**参考来源：**[规范Git提交说明](https://juejin.im/post/6844903831893966856#heading-5)