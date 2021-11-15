# create-svelte

构建 Svelte 项目所需的一切，由 [`create-svelte`](https://github.com/sveltejs/kit/tree/master/packages/create-svelte) 提供支持；

### 创建项目

```bash
# 在当前项目根目录安装
npm init svelte@next

# 创建一个新项目
npm init svelte@next my-app
```

> Note: `@next` 是最新的

### 开发

创建项目并使用 `npm install`（或 `pnpm install` 或 `yarn`）安装依赖项后，启动开发服务器：

```bash
npm run dev

# or 启动服务器并在新的浏览器选项卡中打开应用程序
npm run dev -- --open
```

### 生产

在创建应用的生产版本之前，请为目标环境安装 [适配器](https://kit.svelte.dev/docs#adapters)。 然后：

```bash
npm run build
```

> 无论是否安装了适配器，都可以使用 `npm run preview` 预览构建的应用程序。 这不适用于在生产服务。

# 在 Svelte 项目中使用 StoryBook, 根目录中执行

[err-svelte](https://github.com/sveltejs/kit/issues/2801)

```base
npx sb@next init
```

> .storybook create package.json

```base
{"type":"commonjs"}
```

> .storybook/main.js

```base
module.exports = {
 ... ...

  "svelteOptions": {
     - "preprocess": require("../svelte.config.js").preprocess
     + "preprocess": import("../svelte.config.js").preprocess
  }
}
```

# Svelte 中涉及的非常规知识

> Svelte 本身就是一个编译器，

### 一、反应性能力

1. [反应式-声明](https://svelte.dev/tutorial/reactive-declarations) &nbsp;&nbsp; [label标记语句，`Svelte的第一特性`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/label)

![image](https://user-images.githubusercontent.com/93444868/141734003-db35366b-176e-450b-8f1f-c89700e1507a.png)

2. [反应式-语句](https://svelte.dev/tutorial/reactive-statements)

![image](https://user-images.githubusercontent.com/93444868/141735023-be37296c-17b6-4ea9-a990-86e25c44d0f0.png)

3. [更新数组和对象-Svelte的反应性是由赋值语句触发的](https://svelte.dev/tutorial/updating-arrays-and-objects)

> 相对比较不好理解，需要多练习

### 二、属性 (符合组件层次结构，但与绑定章节有区别)

> 将数据从一个组件向下传递到其子组件

4. [属性传递-道具properties - 使用export关键字，`Svelte的第二特性`](https://svelte.dev/tutorial/declaring-props)

### 三、HTML中的逻辑语块 [一个#字符始终表示块打开标签。一个/字符始终表示块结束标记。](https://svelte.dev/tutorial/else-blocks)

5. [为 each 块添加 key](https://svelte.dev/tutorial/keyed-each-blocks)

```base
{#each things as thing (thing.id)}
	<Thing current={thing.color}/>
{/each}
```

6. [使用 await 处理数据](https:svelte.dev/tutorial/await-blocks)

```base
{#await promise}
	<p>...waiting</p>
{:then number}
	<p>The number is {number}</p>
{:catch error}
	<p style="color: red">{error.message}</p>
{/await}

// or

{#await promise then value}
	<p>the value is {value}</p>
{/await}
```

### 四、事件 使用on:指令监听元素上的任何事件

7. [DOM事件-修饰符](https://svelte.dev/tutorial/event-modifiers)

```base
on:click|once|capture={...}
```

8. [组件事件-createEventDispatcher](https://svelte.dev/tutorial/component-events) 

9. [组件事件-转发](https://svelte.dev/tutorial/event-forwarding)

> 与 DOM 事件不同， 组件事件不会 冒泡（bubble） ，如果想要在某个深层嵌套的组件上监听事件，则中间组件必须 转发（forward） 该事件。

10. [DOM事件-转发](https:svelte.dev/tutorial/dom-event-forwarding)

### 五、[绑定](https://svelte.dev/tutorial/text-inputs)  (符合组件层次结构，但与属性道具 props 章节有区别)

> 相对比较繁杂，需要在实践中多运用. Svelte 中的数据流是自上而下的——父组件可以在子组件上设置 props，组件可以在元素上设置属性，但反过来不行。

> 有时打破这条规则很有用。以<input>这个组件中的元素为例——我们可以添加一个on:input事件处理程序来设置nameto的值event.target.value，但这有点......样板。正如我们将看到的，使用其他表单元素时情况会更糟。

> 相反，我们可以使用bind:value指令

```base
<input bind:value={name}>
```

> 这意味着不仅对值的name更改会更新输入值，而且对输入值的更改也会更新name。

### 六、生命周期  [more](https://svelte.dev/docs#Run_time)

> 虽然在组件初始化期间调用生命周期函数很重要，但从哪里调用它们并不重要。因此，如果我们愿意，我们可以将区间逻辑抽象为一个辅助函数，utils.js

![image](https://user-images.githubusercontent.com/93444868/141764680-d13542b8-ef3c-46f2-8e83-1267389a8d57.png)

### 七、Store 状态容器 (非组件层次结构)

> 并非所有应用程序状态都属于应用程序的`组件层次结构`。有时，值需要由多个不相关的组件或常规 JavaScript 模块访问。在 Svelte 中，使用`Store`来做到这一点。

11. [自动订阅 `$`](https://svelte.dev/tutorial/auto-subscriptions)

> 指定订阅省去了不少的模板代码，而且不需要手动退订

```base

<script>
	import { count } from './stores.js';
	import Incrementer from './Incrementer.svelte';
	import Decrementer from './Decrementer.svelte';
	import Resetter from './Resetter.svelte';

// 	let count_value;

// 	count.subscribe(value => {
// 		count_value = value;
// 	});
</script>

<h1>The count is {$count}</h1>

<Incrementer/>
<Decrementer/>
<Resetter/>

```

12. [readable store](https://svelte.dev/tutorial/readable-stores)

> 我的理解是不需要用户操作的就可更新的只读值称为`readable`，例如 用户地理位置、时间等

13. [drived](https://svelte.dev/tutorial/derived-stores)

> 我的理解是将多个值合并为一个方法返回，称为`derived`派生，可订阅这个方法或者方法里面的值。

14. [custom store - 自定义商店ttps://svelte.dev/tutorial/custom-stores)

15. [store-bind](https://svelte.dev/tutorial/store-bindings)

> store也可以使用绑定

### 八、[Motion运动](https://svelte.dev/tutorial/tweened)

### 九、[transition过渡](https://svelte.dev/tutorial/transition)

### 十、[Animate动画](https://svelte.dev/tutorial/animate)

### 十一、[Action元素级生命周期函数](https://svelte.dev/tutorial/actions)











