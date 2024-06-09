# 入门

有很多方法可以学习如何使用Lit。你可以通过在线测试或者是交互式教程学习，也可以直接安装在已有的工程里面。  

## Lit的在线测试

你可以从在线测试和交互式实例着手学习Lit。你可以从“[Hello World](https://lit.dev/playground/)”这个例子开始，然后试着改一改这个例子，或者继续学习其他的示例。  

## 交互式教程

你还可以跟着我们的[分步交互式教程](https://lit.dev/tutorials/intro-to-lit/)来学习怎么在几分钟之内搭建一个Lit组件。

## Lit例子工程
  
我们还提供TypeScript和JavaScript的例子工程，你可以在它的基础上创建独立的、可重用的组件。参见[例子工程](https://lit.dev/docs/tools/starter-kits/)。

## 使用npm安装到本地

你也可以通过npm来安装Lit，包名称为`lit`。

```
npm i lit
``` 

然后导入到JavaScript或者TypeScript文件中：

```js  

import {LitElement, html} from 'lit'; 
```

```ts
//TypeScript
import {LitElement, html} from 'lit';
import {customElement, property} from 'lit/decorators.js';
```

## 使用打包好的库
Lit也可以作为预编译的、打包好的单文件库来使用。这是为了让开发流程变得更加灵活：比如说你更倾向于下载单一文件，而不是使用npm或者其他的构建工具。举个例子，如果你更愿意下载单个文件，而不是使用npm和构建工具。打包好的库是没有依赖关系的标准JavaScript的模块.所有现代的浏览器都能通过在`<script type="module">`标签里面书写以下代码,从而导入并且运行打包好的库：

```js
import {LitElement, html} from 'https://cdn.jsdelivr.net/gh/lit/dist@2/core/lit-core.min.js';
```
> ❗**如果你使用npm来管理客户端依赖，那么你应该使用[lit包](https://lit.dev/docs/getting-started/#install-locally-from-npm)，而不是打包好的库。** 打包好的库将大部分或者是整个Lit库合并到了一个文件里面，这样的做法会导致你的页面下载不必要的代码。   

要浏览这些打包好的库，你可以去[https://cdn.jsdelivr.net/gh/lit/dist/](https://cdn.jsdelivr.net/gh/lit/dist/)页面，然后使用下拉菜单选择版本，从而跳转到特定版本的页面中。在那个页面中，你可以找到一个目录，列出了所选版本中所有可用的打包好的库的类型。有两种类型的打包好的库：

|Bundle Type|Description |  
| ----------- | ----------- |  
|**`core`**   |[https://cdn.jsdelivr.net/gh/lit/dist@2/core/lit-core.min.js](https://cdn.jsdelivr.net/gh/lit/dist@2/core/lit-core.min.js)<br/>`core`导出了跟[Lit包的主模块](https://github.com/lit/lit/blob/main/packages/lit/src/index.ts)相同的项目。|   
|**`all`** | [https://cdn.jsdelivr.net/gh/lit/dist@2/all/lit-all.min.js](https://cdn.jsdelivr.net/gh/lit/dist@2/all/lit-all.min.js)<br/>`all`除了导出了`core`里面的[所有项目](https://github.com/lit/lit/blob/main/packages/lit/src/index.all.ts)以外，还导出了许多lit中大多数其它模块。 | 

## 添加Lit到一个现有的项目

关于如何把Lit添加到[添加Lit到一个现有的项目示例](https://lit.dev/docs/tools/adding-lit/)的指南。  

## Open WC的项目生成器

Open WC项目有一个[项目生成器](https://open-wc.org/docs/development/generator/)，Lit使用它来架构出一个应用程序项目。  

