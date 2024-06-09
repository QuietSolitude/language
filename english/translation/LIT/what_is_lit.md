# Lit是什么
Lit是一个简单易用的，用来构建快速、轻量化的web component的库。 

Lit的核心是一个用于构建组件的基类，这个基类提供了响应式的状态、作用域明晰的样式和一个描述性的模板系统，它还非常小巧、快速，并且有很强的表达能力，从而可以帮助我们砍掉工程中大量的重复性代码。

## 我能用Lit做什么？
不管是什么样的Web UI，你都可以用Lit来构建！

有关于Lit，你需要知道的第一件事情是**每一个Lit组件都是一个标准的[Web Component](https://developer.mozilla.org/zh-CN/docs/Web/Web_Components)**。Web Component具有强大的互通性：它不仅可以被浏览器原生支持，不管有没有框架、有什么框架，它都可以在任何HTML环境下使用。

用Lit来开发**共享组建或者是设计系统**是一个理想的选择。就算是不同的应用和网站使用的前端技术完全不同，我们也可以在这些应用和网站里使用Lit构建的组件。网站开发者在调用Lit组件的时候不用写Lit有关的代码，其实连看Lit有关的代码都没必要。他们只要像调用内置的HTML元素一样来调用Lit的组件就可以了。

Lit也非常适合**在基础的网站上逐步增强**，不管你的网站是手写的，还是通过内容管理系统写的，还是通过服务器端框架构建的，还是用Jekyll或者是eleventy生成的，浏览器都可以识别标记语言中的Lit组件，并且自动初始化他们。 

当然了，你也可以用Lit组件来构建**高交互性、功能丰富的应用程序**，这跟你用React或者Vue之类的框架来构建的效果是一样的。Lit的能力和开发者的经验跟当下流行的前端开发框架旗鼓相当，通过采用浏览器的原生组件模型，Lit把开发者对它的依赖减到最小，把框架的灵活性放到最大，并且让可维护性上升到了一个新的高度。

在你用Lit构建应用的时候，不管你是想加点“原味”的web component也好，还是想要加点用其他的库写的web component也好，都是相当轻松的一件事。
你甚至能在不中止开发的情况下一次一个组件地升级Lit的一个新的主要版本号，或者是迁移到另一个库。如果你有过一些现代的、基于组件的web开发经验的话，你应该就会觉得Lit用起来就像是在主场作战一样.

## 怎么使用lit来开发？
如果你使用过现代的、基于组件的方式进行过web开发，你会感觉到Lit非常熟悉。哪怕你以前没用过组件进行过开发也不要紧，我们相信你会发现Lit是非常容易上手的。

每个Lit组件都是一个独立的UI单元，是由较小的标准HTML元素或者其他一些web component组装而成的。然而，每个Lit component本身进而又是其他部分的组成单元，可以在HTML文档中，在另外一个web component或者在一个框架组件中使用，来构建大而更复杂的界面。

这里我们以一个简单的小组件（一个倒计时器）为例，来介绍Lit代码是什么样子的，并且介绍它的关键特征。

```ts
//my-timer.ts
import {LitElement, html, css} from 'lit';
import {customElement, property, state} from 'lit/decorators.js';
/* playground-fold */
import {play, pause, replay} from './icons.js';
/* playground-fold-end */

@customElement("my-timer")
export class MyTimer extends LitElement {
  static styles = css`/* playground-fold */

    :host {
      display: inline-block;
      min-width: 4em;
      text-align: center;
      padding: 0.2em;
      margin: 0.2em 0.1em;
    }
    footer {
      user-select: none;
      font-size: 0.6em;
    }
    /* playground-fold-end */`;

  @property() duration = 60;
  @state() private end: number | null = null;
  @state() private remaining = 0;

  render() {
    const {remaining, running} = this;
    const min = Math.floor(remaining / 60000);
    const sec = pad(min, Math.floor(remaining / 1000 % 60));
    const hun = pad(true, Math.floor(remaining % 1000 / 10));
    return html`
      ${min ? `${min}:${sec}` : `${sec}.${hun}`}
      <footer>
        ${remaining === 0 ? '' : running ?
          html`<span @click=${this.pause}>${pause}</span>` :
          html`<span @click=${this.start}>${play}</span>`}
        <span @click=${this.reset}>${replay}</span>
      </footer>
    `;
  }
  /* playground-fold */

  start() {
    this.end = Date.now() + this.remaining;
    this.tick();
  }

  pause() {
    this.end = null;
  }

  reset() {
    const running = this.running;
    this.remaining = this.duration * 1000;
    this.end = running ? Date.now() + this.remaining : null;
  }

  tick() {
    if (this.running) {
      this.remaining = Math.max(0, this.end! - Date.now());
      requestAnimationFrame(() => this.tick());
    }
  }

  get running() {
    return this.end && this.remaining;
  }

  connectedCallback() {
    super.connectedCallback();
    this.reset();
  }/* playground-fold-end */

}
/* playground-fold */

function pad(pad: unknown, val: number) {
  return pad ? String(val).padStart(2, '0') : val;
}/* playground-fold-end */


```
```html
<!-- index.html -->
<div>
  <!doctype html>
  <head><!-- playground-fold -->
  <link rel="preconnect" href="https://fonts.gstatic.com">
  <link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:ital,wght@1,800&display=swap" rel="stylesheet">
  <script type="module" src="./my-timer.js"></script>
  <style>
    body {
      font-family: 'JetBrains Mono', monospace;
      font-size: 36px;
    }
  </style>
  <!-- playground-fold-end -->
</head>
<body>
  <my-timer duration="7"></my-timer>
  <my-timer duration="60"></my-timer>
  <my-timer duration="300"></my-timer>
</body>
44
</div>
```
```ts
//icons.ts
import {html} from 'lit';

export const replay = html`<svg xmlns="http://www.w3.org/2000/svg" enable-background="new 0 0 24 24" height="24px" viewBox="0 0 24 24" width="24px" fill="#000000"><title>Replay</title><g><rect fill="none" height="24" width="24"/><rect fill="none" height="24" width="24"/><rect fill="none" height="24" width="24"/></g><g><g/><path d="M12,5V1L7,6l5,5V7c3.31,0,6,2.69,6,6s-2.69,6-6,6s-6-2.69-6-6H4c0,4.42,3.58,8,8,8s8-3.58,8-8S16.42,5,12,5z"/></g></svg>`;
export const pause = html`<svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 0 24 24" width="24px" fill="#000000"><title>Pause</title><path d="M0 0h24v24H0V0z" fill="none"/><path d="M6 19h4V5H6v14zm8-14v14h4V5h-4z"/></svg>`;
export const play = html`<svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 0 24 24" width="24px" fill="#000000"><title>Play</title><path d="M0 0h24v24H0V0z" fill="none"/><path d="M10 8.64L15.27 12 10 15.36V8.64M8 5v14l11-7L8 5z"/></svg>`;

```
一些需要注意的地方：

* Lit主要的特性是基类`LitElement`，它是原生`HTMLElement`的扩展，非常易于使用并且功能丰富。你可以通过扩展LitElement来定义你自己的组件
* Lit **[富有表达性的声明性模板](https://lit.dev/docs/templates/overview/)**（是利用JavaScript标记文字的模板）使描述组件的呈现方式变得容易。
* **[响应式属性](https://lit.dev/docs/components/properties/)** 描述一个组件的公共API以及内部状态；在响应式属性发生变化的时候，你的组件也会自动发生变化。
* **[样式](https://lit.dev/docs/components/styles/)** 在默认情况下是有一个限定的作用域的，在这个作用域的环境中可以让你的CSS选择器保持简单和确保你的组件样式不会污染周围的环境或者是被周围环境所污染。
* Lit能在在基础的JavaScript中很好的完成工作，或者你甚至可以通过使用TypeScript的修饰器和声明类型来实现更好的人体工程学。

Lit在开发期间是不需要编译和构建的，如果你喜欢的话，Lit几乎不需要使用工具。有着一流的 **[IDE Support](https://lit.dev/docs/tools/development/#ide-plugins)** （代码完成，linting等等）和 **[生产工具](https://lit.dev/docs/tools/production/)**（本地化，模板缩小等）都是随时可用。

## 为什么我要选择Lit？
我们已经注意到了，Lit是构建各种web UI的最佳选择，它具有Web component的互通性的优势和现代化，人体工程学的开发经验。

Lit还有以下优点：

* **简单的**：构建在标准Web component之上，Lit添加了你需要的轻松和高效的东西：反应性，声明性模板和一些成熟的减少样板的功能使你工作更简单。
* **快速的**：更新速度很快，因为Lit对你的UI动态部分保持跟踪，当这部分底层状态改变时，才会对这部分更新，它不需要先用整个虚拟dom树状结构跟已有的dom状态进行比较，然后重新构建整个虚拟dom的树状结构。
* **轻量化**：Lit有助于大概只有5k大小（最小化和压缩后）的代码打包和快速加载。

Lit的背后团队从第一天被卷入了Web Components。我们帮助Google维护成千上万的组件，帮助提供一个全面集合的Web Components polyfills，帮助参与深入标准和社区工作。  

每个Lit的新特性被设计出来都想着浏览器技术的演变；我们可以帮助你充分利用当今平台提供的所有优势，这样你写的代码就可以随着技术未来的发展而不断获益，这就是我们的目标。  

## 下一步

* **[入门](https://lit.dev/docs/getting-started/)**：设置好Lit和使用Lit去开发。  
* **[Component](https://lit.dev/docs/components/overview/)**：学习有关Lit的component model的知识。
* **[模板](https://lit.dev/docs/templates/overview/)**：用Lit的HTML语法来写模版。
* **[代码组织](https://lit.dev/docs/composition/overview/)**：编写可以重复使用和可以维护的代码。