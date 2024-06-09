# 组件的概述

一个Lit component是一个可重复使用的UI部分。你可以把一个Lit component看作一个容器，它会显示一些基于UI的状态信息。它也能对用户输入、触发事件——任何你期望UI component能做的事情做出反应。而且Lit component 是一个HTML元素，所以它包含了所有标准的元素的API。  

创建一个Lit component涉及的一些概念：

* [定义一个component](https://lit.dev/docs/components/defining/)。一个Lit component是 _自定义元素_ 实现，并在浏览器中注册。
* [渲染](https://lit.dev/docs/components/rendering/)。一个组件有一个 _render方法_，调用这个方法可来渲染组件的内容。在这个render方法中，你可以为该组件定义一个模板。
* [反应属性](https://lit.dev/docs/components/properties/)。属性保留这个组件的状态，替换一个或者多个组件的 _反应属性_ 触发一个更新循环，重新渲染组件。
* [样式](https://lit.dev/docs/components/styles/)。一个组件能定义 _封装的样式_ 来控制它自身的外观。
* [生命周期](https://lit.dev/docs/components/lifecycle/)。Lit定义了一组回调函数，你可以覆盖这些回调函数并挂接到组件的生命周期中，举个例子：将元素添加到一个页面的时候、或者组件更新的时候运行一段代码。

这里有一个简单的组件例子：  
```ts
import {LitElement, css, html} from 'lit';
import {customElement, property} from 'lit/decorators.js';

@customElement('simple-greeting')
export class SimpleGreeting extends LitElement {
  // Define scoped styles right with your component, in plain CSS
  static styles = css`
    :host {
      color: blue;
    }
  `;

  // Declare reactive properties
  @property()
  name?: string = 'World';

  // Render the UI as a function of component state
  render() {
    return html`<p>Hello, ${this.name}!</p>`;
  }
}
```
```js
import {LitElement, css, html} from 'lit';

export class SimpleGreeting extends LitElement {
  static properties = {
    name: {},
  };
  // Define scoped styles right with your component, in plain CSS
  static styles = css`
    :host {
      color: blue;
    }
  `;

  constructor() {
    super();
    // Declare reactive properties
    this.name = 'World';
  }

  // Render the UI as a function of component state
  render() {
    return html`<p>Hello, ${this.name}!</p>`;
  }
}
customElements.define('simple-greeting', SimpleGreeting);

```