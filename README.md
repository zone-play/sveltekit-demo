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

1. [反应式声明](https://www.sveltejs.cn/tutorial/reactive-declarations) &nbsp;&nbsp; [label标记语句](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/label)

![image](https://user-images.githubusercontent.com/93444868/141734003-db35366b-176e-450b-8f1f-c89700e1507a.png)

2. [反应式语句](https://www.sveltejs.cn/tutorial/reactive-statements)

![image](https://user-images.githubusercontent.com/93444868/141735023-be37296c-17b6-4ea9-a990-86e25c44d0f0.png)



