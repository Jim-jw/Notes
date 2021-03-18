# Vue风格指南跟规范

### 规则归类
- **优先级 A：必要的**
  > 这些规则会帮你规避错误，所以学习并接受它们带来的全部代价吧。这里面可能存在例外，但应该非常少，且只有你同时精通 JavaScript 和 Vue 才可以这样做。
- **优先级 B：强烈推荐**
  > 这些规则能够在绝大多数工程中改善可读性和开发体验。即使你违反了，代码还是能照常运行，但例外应该尽可能少且有合理的理由。
- **优先级 C：推荐**
  > 当存在多个同样好的选项，选任意一个都可以确保一致性。在这些规则里，我们描述了每个选项并建议一个默认的选择。也就是说只要保持一致且理由充分，你可以随意在你的代码库中做出不同的选择。请务必给出一个好的理由！通过接受社区的标准，你将会：
  >  1. 训练你的大脑，以便更容易的处理你在社区遇到的代码；
  >  2. 不做修改就可以直接复制粘贴社区的代码示例；
  >  3. 能够经常招聘到和你编码习惯相同的新人，至少跟 Vue 相关的东西是这样的。
- **优先级 D：谨慎使用**
  > 有些 Vue 特性的存在是为了照顾极端情况或帮助老代码的平稳迁移。当被过度使用时，这些特性会让你的代码难于维护甚至变成 bug 的来源。这些规则是为了给有潜在风险的特性敲个警钟，并说明它们什么时候不应该使用以及为什么。

### 1. 组件名为多个单词(A)
> 组件名应该始终是多个单词的，根组件 `App` 以及 `<transition>`、`<component>` 之类的 Vue 内置组件除外。

这样做可以避免跟现有的以及未来的 HTML 元素相冲突，因为所有的 HTML 元素名称都是单个单词的。

**坏:chestnut:**
```
Vue.component('todo', {
  // ...
})
export default {
  name: 'Todo',
  // ...
}
```
**好:chestnut:**
```
Vue.component('todo-item', {
  // ...
})
export default {
  name: 'TodoItem',
  // ...
}
```

### 2. 避免 `v-if` 和 `v-for` 用在一起(A)

**永远不要把 v-if 和 v-for 同时用在同一个元素上。**

一般我们在两种常见的情况下会倾向于这样做：

- 为了过滤一个列表中的项目 (比如 `v-for="user in users" v-if="user.isActive"`)。在这种情形下，请将 users 替换为一个计算属性 (比如 `activeUsers`)，让其返回过滤后的列表。

- 为了避免渲染本应该被隐藏的列表 (比如 `v-for="user in users" v-if="shouldShowUsers"`)。这种情形下，请将 `v-if` 移动至容器元素上 (比如 ul、ol)。

**详解：**
当 Vue 处理指令时，`v-for` 比 `v-if` 具有更高的优先级，所以这个模板
```
<ul>
  <li
    v-for="user in users"
    v-if="user.isActive"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>
```
将会经过如下运算：
```
this.users.map(function (user) {
  if (user.isActive) {
    return user.name
  }
})
```
因此哪怕我们只渲染出一小部分用户的元素，也得在每次重渲染的时候遍历整个列表，不论活跃用户是否发生了变化。

通过将其更换为在如下的一个计算属性上遍历：
```
computed: {
  activeUsers: function () {
    return this.users.filter(function (user) {
      return user.isActive
    })
  }
}
```
```
<ul>
  <li
    v-for="user in activeUsers"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>
```
我们将会获得如下好处：

过滤后的列表只会在 `users` 数组发生相关变化时才被重新运算，过滤更高效。
使用 `v-for="user in activeUsers"` 之后，我们在渲染的时候只遍历活跃用户，渲染更高效。
解耦渲染层的逻辑，可维护性 (对逻辑的更改和扩展) 更强。
为了获得同样的好处，我们也可以把：
```
<ul>
  <li
    v-for="user in users"
    v-if="shouldShowUsers"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>
```
更新为：
```
<ul v-if="shouldShowUsers">
  <li
    v-for="user in users"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>
```
通过将 `v-if` 移动到容器元素，我们不会再对列表中的每个用户检查 `shouldShowUsers`。取而代之的是，我们只检查它一次，且不会在 `shouldShowUsers` 为否的时候运算 `v-for`。

### 3. 单文件组件文件的大小写(B)
**单文件组件**的文件名应该要么始终是单词大写开头 (PascalCase)，要么始终是横线连接 (kebab-case)。

单词大写开头对于代码编辑器的自动补全最为友好，因为这使得我们在 JS(X) 和模板中引用组件的方式尽可能的一致。然而，混用文件命名方式有的时候会导致大小写不敏感的文件系统的问题，这也是横线连接命名同样完全可取的原因。

**坏:chestnut:**
```
components/
|- mycomponent.vue
|- myComponent.vue
```
**好:chestnut:**
```
components/
|- MyComponent.vue
|- my-component.vue
```

### 4. 基础组件名(B)
应用特定样式和约定的基础组件 (也就是展示类的、无逻辑的或无状态的组件) 应该全部以一个特定的前缀开头，比如 `Base`、`App` 或 `V`。

**坏:chestnut:**
```
components/
|- MyButton.vue
|- VueTable.vue
|- Icon.vue
```
**好:chestnut:**
```
components/
|- BaseButton.vue
|- BaseTable.vue
|- BaseIcon.vue
```

### 5. 单例组件名(B)

**只应该拥有单个活跃实例的组件应该以 The 前缀命名，以示其唯一性。**

这不意味着组件只可用于一个单页面，而是每个页面只使用一次。这些组件永远不接受任何 prop，因为它们是为你的应用定制的，而不是它们在你的应用中的上下文。如果你发现有必要添加 prop，那就表明这实际上是一个可复用的组件，只是目前在每个页面里只使用一次。

**坏:chestnut:**
```
components/
|- Heading.vue
|- MySidebar.vue
```
**好:chestnut:**
```
components/
|- TheHeading.vue
|- TheSidebar.vue
```

### 6. 紧密耦合的组件名(B)
**和父组件紧密耦合的子组件应该以父组件名作为前缀命名。**

如果一个组件只在某个父组件的场景下有意义，这层关系应该体现在其名字上。因为编辑器通常会按字母顺序组织文件，所以这样做可以把相关联的文件排在一起。

**坏:chestnut:**
```
components/
|- TodoList.vue
|- TodoItem.vue
|- TodoButton.vue
```
**好:chestnut:**
```
components/
|- TodoList.vue
|- TodoListItem.vue
|- TodoListItemButton.vue
```

### 7. 组件名中的单词顺序
**组件名应该以高级别的 (通常是一般化描述的) 单词开头，以描述性的修饰词结尾。**

**坏:chestnut:**
```
components/
|- ClearSearchButton.vue
|- ExcludeFromSearchInput.vue
|- LaunchOnStartupCheckbox.vue
|- RunSearchButton.vue
|- SearchInput.vue
|- TermsCheckbox.vue
```
**好:chestnut:**
```
components/
|- SearchButtonClear.vue
|- SearchButtonRun.vue
|- SearchInputQuery.vue
|- SearchInputExcludeGlob.vue
|- SettingsCheckboxTerms.vue
|- SettingsCheckboxLaunchOnStartup.vue
```

### 8. 自闭合组件(B)
**在单文件组件、字符串模板和 JSX 中没有内容的组件应该是自闭合的——但在 DOM 模板里永远不要这样做。**

自闭合组件表示它们不仅没有内容，而且刻意没有内容。其不同之处就好像书上的一页白纸对比贴有“本页有意留白”标签的白纸。而且没有了额外的闭合标签，你的代码也更简洁。

不幸的是，HTML 并不支持自闭合的自定义元素——只有官方的“空”元素。所以上述策略仅适用于进入 DOM 之前 Vue 的模板编译器能够触达的地方，然后再产出符合 DOM 规范的 HTML。

**坏:chestnut:**
```
<!-- 在单文件组件、字符串模板和 JSX 中 -->
<MyComponent></MyComponent>
<!-- 在 DOM 模板中 -->
<my-component/>
```
**好:chestnut:**
```
<!-- 在单文件组件、字符串模板和 JSX 中 -->
<MyComponent/>
<!-- 在 DOM 模板中 -->
<my-component></my-component>
```

### 9. 模板中的组件名大小写(B)
**对于绝大多数项目来说，在单文件组件和字符串模板中组件名应该总是 PascalCase 的——但是在 DOM 模板中总是 kebab-case 的。**

PascalCase 相比 kebab-case 有一些优势：

- 编辑器可以在模板里自动补全组件名，因为 PascalCase 同样适用于 JavaScript。
- `<MyComponent>` 视觉上比 `<my-component>` 更能够和单个单词的 HTML 元素区别开来，因为前者的不同之处有两个大写字母，后者只有一个横线。
- 如果你在模板中使用任何非 Vue 的自定义元素，比如一个 Web Component，PascalCase 确保了你的 Vue 组件在视觉上仍然是易识别的。
不幸的是，由于 HTML 是大小写不敏感的，在 DOM 模板中必须仍使用 kebab-case。

还请注意，如果你已经是 kebab-case 的重度用户，那么与 HTML 保持一致的命名约定且在多个项目中保持相同的大小写规则就可能比上述优势更为重要了。在这些情况下，在所有的地方都使用 kebab-case 同样是可以接受的。

**坏:chestnut:**
```
<!-- 在单文件组件和字符串模板中 -->
<mycomponent/>
<!-- 在单文件组件和字符串模板中 -->
<myComponent/>
<!-- 在 DOM 模板中 -->
<MyComponent></MyComponent>
```
**好:chestnut:**
```
<!-- 在单文件组件和字符串模板中 -->
<MyComponent/>
<!-- 在 DOM 模板中 -->
<my-component></my-component>
<!-- 在所有地方 -->
<my-component></my-component>
```

### 10. 组件/实例的选项的顺序(C)
**组件/实例的选项应该有统一的顺序。**

这是我们推荐的组件选项默认顺序。它们被划分为几大类，所以你也能知道从插件里添加的新 property 应该放到哪里。
  - 全局感知 (要求组件以外的知识)
    - `name`
  - 模板依赖(模板内使用的资源)
    - `conponents`
    - `directives`
    - `filters`
  - 组合(向选项里合并 property)
    - `extends`
    - `mixins`
  - 接口(组件的接口)
    - `model`
    - `props`
  - 本地状态(本地的响应式property)
    - `data`
    - `computed`
  - 事件(通过响应式事件触发的回调)
    - `watch`
    - 生命周期钩子
      - `create`
      - `mounted`
      - `beforeDestroy` （业务代码关于卸载相关周期钩子我建议是放在最后，大部分没有review的必要）
  - 非响应式的 property(不依赖响应系统的实例 property)
    - `methods`


### 11. 元素 attribute 的顺序(C)
**元素 (包括组件) 的 attribute 应该有统一的顺序。**
  - 定义(提供组件的选项)
    - `is`
  - 列表渲染(创建多个变化的相同元素)
    - `v-for`
  - 条件渲染(元素是否渲染/显示)
    - `v-if`
    - `v-else-if`
    - `v-else`
    - `v-show`
    - `v-cloak`
  - 渲染方式(改变元素的渲染方式)
    - `v-pre`
    - `v-once`
  - 全局感知(需要超越组件的知识)
    - `id`
  - 唯一的attribute (需要唯一值的 attribute)
    - `ref`
    - `key`
  - 双向绑定(把绑定和事件结合起来)
    - `v-model`
  - 其他attribute(所有普通的绑定或未绑定的attribute)
  - 事件(组件事件监听器)
    - `v-on`
  - 内容(覆写元素的内容)
    - `v-html`
    - `v-text`


### 12. 没有在 `v-if`/`v-else-if`/`v-else` 中使用 `key`(D)
**如果一组 `v-if` + `v-else` 的元素类型相同，最好使用 `key` (比如两个 `<div>` 元素)。**

默认情况下，Vue 会尽可能高效的更新 DOM。这意味着其在相同类型的元素之间切换时，会修补已存在的元素，而不是将旧的元素移除然后在同一位置添加一个新元素。如果本不相同的元素被识别为相同，则会出现意料之外的结果。

### 13. `scoped` 中的元素选择器(D)
**元素选择器应该避免在 `scoped` 中出现。**

在 scoped 样式中，类选择器比元素选择器更好，因为大量使用元素选择器是很慢的。

**详解：**
为了给样式设置作用域，Vue 会为元素添加一个独一无二的 attribute，例如 data-v-f3f3eg9。然后修改选择器，使得在匹配选择器的元素中，只有带这个 attribute 才会真正生效 (比如 button[data-v-f3f3eg9])。

问题在于大量的`元素和 attribute 组合的选择器` (比如 button[data-v-f3f3eg9]) 会比`类和 attribute 组合的选择器`慢，所以应该尽可能选用类选择器。

### 14. 隐性的父子组件通信(D)
**应该优先通过 prop 和事件进行父子组件之间的通信，而不是 `this.$parent` 或变更 prop。**

一个理想的 Vue 应用是 prop 向下传递，事件向上传递的。遵循这一约定会让你的组件更易于理解。然而，在一些边界情况下 prop 的变更或 `this.$parent` 能够简化两个深度耦合的组件。

问题在于，这种做法在很多简单的场景下可能会更方便。但请当心，不要为了一时方便 (少写代码) 而牺牲数据流向的简洁性 (易于理解)。


### 15. 业务代码或者复杂组件推荐用文件夹 + index.vue/.js/.ts 文件结构
举个 :chestnut:
```
TheHeader
  |-index.vue
  |-components
    |-left.vue
    |-right.vue
```

### 16. `Vue2`中尽量少用或不用`mixins`
  1. `mixins`引入了隐式依赖关系,不利于代码阅读跟问题排查
  2. 不同`mixins`之间可能会有代码冲突覆盖的问题
  3. 3 `mixins`代码会导致滚雪球式的复杂性


---
**参考来源：**[Vue官网-风格指南](https://cn.vuejs.org/v2/style-guide/)