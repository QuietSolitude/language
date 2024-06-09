# 渲染
添加一个模板到你的组件里定义模板要渲染的内容。模板能包含表达式，它们是占位符的动态内容。  

在Lit组件里定义一个模板，在里面添加一个render()方法。  

```ts
import {LitElement, html} from 'lit';
import {customElement} from 'lit/decorators.js';

@customElement('my-element')
class MyElement extends LitElement {

  render(){
    return html`<p>Hello from my template.</p>`;
  }
}
```
```js
import {LitElement, html} from 'lit';

class MyElement extends LitElement {
  render() {
    return html`<p>Hello from my template.</p>`;
  }
}
customElements.define('my-element', MyElement);
```
Lit的[html](https://lit.dev/docs/api/templates/#html)标记函数，是使用JavaScript[标记模板文字](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals#tagged_templates)中使用HTML语言来编写你的模板。  

Lit模板能包含JavaScrip _表达式_。你可以使用表达式来设置文本内容，特性，属性和事件监听。`render()`方法还可以包含任何JavaScript——例如，你能创建局部变量可以在表达式中使用。

## 可渲染值
通常，组件的`render()`方法返回单个`TemplateResult`对象（和通过`html`标记函数返回的类型是相同的）。然而，它可以返回Lit可以渲染的、可以作为HTML元素的子元素的所有东西：

* 原始值如字符串，数值，还有布尔值
* 通过`html`函数来创建`TemplateResult`对象。
* DOM节点。
* 