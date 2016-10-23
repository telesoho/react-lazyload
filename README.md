# react-lazyload [![Build Status](https://travis-ci.org/jasonslyvia/react-lazyload.svg)](https://travis-ci.org/jasonslyvia/react-lazyload) [![npm version](https://badge.fury.io/js/react-lazyload.svg)](http://badge.fury.io/js/react-lazyload) [![Coverage Status](https://coveralls.io/repos/github/jasonslyvia/react-lazyload/badge.svg?branch=master)](https://coveralls.io/github/jasonslyvia/react-lazyload?branch=master)

可以对组件、图片或者任何其他的内容执行懒加载。

[Online Demo](//jasonslyvia.github.io/react-lazyload/examples/)

## 它为什么更好

 - 只需要监听两个事件久可以实现对所有的组件进行懒加载
 - 支持同时懒加载`one-time lazy load`和持续懒加载`continuous lazy load`两种模式
 - 对`scroll` / `resize`事件处理进行了调节避免频繁刷新， 还可以切换到debounce模式
 - 支持Decorator
 - 完全测试

## 安装方法

> 2.0.0已经发布, 请参考[Upgrade Guide](https://github.com/jasonslyvia/react-lazyload/wiki/Upgrade-Guide)进行升级!

```
$ npm install --save react-lazyload
```

## 使用方法

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import LazyLoad from 'react-lazyload';
import MyComponent from './MyComponent';

const App = () => {
  return (
    <div className="list">
      <LazyLoad height={200}>
        <img src="tiger.jpg" /> /*
                                  Lazy loading images is supported out of box,
                                  no extra config needed, set `height` for better
                                  experience
                                 */
      </LazyLoad>
      <LazyLoad height={200} once >        
                                /* Once this component is loaded, LazyLoad will
                                 not care about it anymore, set this to `true`
                                 if you're concerned about improving performance */
        <MyComponent />
      </LazyLoad>
      <LazyLoad height={200} offset={100}>
                              /* This component will be loaded when it's top
                                 edge is 100px from viewport. It's useful to
                                 make user ignorant about lazy load effect. */
        <MyComponent />
      </LazyLoad>
      <LazyLoad>
        <MyComponent />
      </LazyLoad>
    </div>
  );
};

ReactDOM.render(<App />, document.body);
```

如果你想让组件默认进行懒加载，可以用下面的方便的decorator模式:

```javascript
import { lazyload } from 'react-lazyload';

@lazyload({
  height: 200,
  once: true,
  offset: 100
})
class MyComponent extends React.Component {
  render() {
    return <div>this component is lazyloaded by default!</div>;
  }
}
```

需要注意的是你的组件只有在可视范围内并且占位符被渲染时才会被加载。 
所以可以在组件的`componentDidMount`状态下安全的发送请求而不必担心损失性能，或者可以加入一些很酷的进入特效。可参考[示例](https://jasonslyvia.github.io/react-lazyload/examples/#/fadein)了解更多细节。

## Props

### height

Type: Number/String Default: undefined

第一次渲染时，LazyLoad将渲染占位符，并测试组件是否可视。设置`height`属性可以让LazyLoad的计算更加精确，比如数字或类似字符串`'100%'`。设置height属性，不使用`height`属性，而是使用css设置占位符的高度。

### once

Type: Bool Default: false

一旦懒加载组件被加载后，不在监听scroll/resize事件。通常对图片和简单组件使用这个选项。

### offset

Type: Number/Array(Number) Default: 0

如果你想组件在低于视图100px的时候被加载（用户必须多滚动100px才能看到该组件），可以设置`offset`属性为`100`。另外，你也可以通过设置`offset`为负数来延迟加载组件的甚至组件顶部已经进入视图。

如果你设置属性为`[200, 200]`, 它将设置顶部边界便宜和底部边界偏移。

### scroll

Type: Bool Default: true

Listen and react to scroll event.

### resize

Type: Bool Default: false

Respond to `resize` event, set it to `true` if you do need LazyLoad listen resize event.

**NOTICE** If you tend to support legacy IE, set this props carefully, refer to [this question](http://stackoverflow.com/questions/1852751/window-resize-event-firing-in-internet-explorer) for further reading.

### overflow

Type: Bool Default: false

If lazy loading components inside a overflow container, set this to `true`. Also make sure a `position` property other than `static` has been set to your overflow container.

[demo](https://jasonslyvia.github.io/react-lazyload/examples/#/overflow)

### debounce

Type: Bool / Number Default: true

By default, LazyLoad will have all event handlers debounced in 300ms for better performance. You can disable this by setting `debounce` to `false`, or change debounce time by setting a number value.

[demo](https://jasonslyvia.github.io/react-lazyload/examples/#/debounce)

### throttle

Type: Bool / Number Default: false

If you prefer `throttle` rather than `debounce`, you can set this props to `true` or provide a specific number.

**NOTICE** Set `debounce` / `throttle` to all lazy loaded components unanimously, if you don't, the first occurrence is respected.

### placeholder

Type: Any Default: undefined

Specify a placeholder for your lazy loaded component.

[demo](https://jasonslyvia.github.io/react-lazyload/examples/#/placeholder)

**If you provide your own placeholder, do remember add appropriate `height` or `minHeight` to your placeholder element for better lazyload performance.**

## Utility

### forceCheck

This is avaible to manually trigger checking for elements in viewport. Helpful when LazyLoad compoents enter the viewport without resize or scroll events, e.g. when the components' container was hidden then become visible.

Import `forceCheck`:

```javascript

import {forceCheck} from 'react-lazyload';

```

Then call the function:

```javascript

forceCheck();

```


## Scripts

```
$ npm run demo:watch
$ npm run build
```

## Who should use it

Let's say there is a `fixed` date picker on the page, when user pick a different date, all components displaying data should send ajax request with new date parameter to retreive updated data, even many of them aren't visible in viewport. This makes server load furious when there are too many requests in one page.

Using `LazyLoad` component will help ease this situation by only update components in viewport.

## Contributors

1. [lancehub](https://github.com/lancehub)
2. [doug-wade](https://github.com/doug-wade)


## License

MIT
