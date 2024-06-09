# 定义一个组件
通过创建一个类继承LitElement并向浏览器注册这个类来定义一个Lit Component。
```
@customElement('simple-greeting')
export class SimpleGreeting extends LitElement { /* ... */ }
```
这个@customElement 装饰器是调用[cstomElement.define](https://developer.mozilla.org/en-US/docs/Web/API/CustomElementRegistry/define)的简写，customElement.define向浏览器注册一个自定义元素类并关联一个元素名(在这里例子中，元素名是simple-greeting)。
如果你使用的是JavaScript，或者你没有使用装饰器，你可以直接调用define()函数：
```
export class SimpleGreeting extends LitElement { /* ... */  }
customElements.define('simple-greeting', SimpleGreeting);
```
## 一个Lit component 是一个HTML元素
你定义一个Lit component的时候，你就是定义一个[自定义HTML元素](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Using_custom_elements)。所以你能像使用一些内置的元素一样使用新的元素:
```
<simple-greeting name="Markup"></simple-greeting>
```
```
const greeting = document.createElement('simple-greeting');
```
基类`LitElement`是`HTMLElement`的一个子类，所以一个Lit component继承了标准的`HTMLElement`的所有属性和方法。

具体来说，`LitElement`是继承`ReactiveElement`的，`ReactiveElement`实现了反应属性，并且继继承了`HTMLElement`类。
![例子图](https://lit.dev/images/docs/components/lit-element-inheritance.png)

## 提供良好的TypeScript类型

TypeScript会推断类返回基于标签名字上某个DOM APIs一个HTML元素。举个例子，`document.createElement('img')`返回一个带有`src: string`
属性的`HTMLImageElement`的实例。

自定义元素能通过如下添加`HTMLElementTagNameMap`来获取相同的处理：
```ts
@customElement('my-element')
export class MyElement extends LitElement {
  @property({type: Number})
  aNumber: number = 5;
  /* ... */
}

declare global {
  interface HTMLElementTagNameMap {
    "my-element": MyElement;
  }
}
```

通过这么做，接下来的代码判断类型检查是否正确。
```ts
const myElement = document.createElement('my-element');
myElement.aNumber = 10;
```
我们建议在TypeScript中编写所有元素中添加一个`HTMLElementTagNameMap`选项，并确保在你的npm包里有你发布`.d.ts`类型。