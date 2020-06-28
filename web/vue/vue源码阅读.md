[TOC]



# Vue源码阅读笔记

## 1. 入口

package.json中 可看到 build为scripts/build.js

### 1.1 构建（build.js）

在build.js 中主要是执行了 build函数，传入了builds

```js
let builds = require('./config').getAllBuilds()
···
build(builds)
···
```

getAllBuilds() 获取了所有的Vue构建的版本，并根据配置文件获取对应的版本进行构建

```
if (process.env.TARGET) {
  module.exports = genConfig(process.env.TARGET)
} else {
  exports.getBuild = genConfig
  exports.getAllBuilds = () => Object.keys(builds).map(genConfig)
}
```
config.js中 我们看第一个入口为('web/entry-runtime.js')

这里 web是别名配置在 alias.js中 为 “src/platforms/web”

```
'web-runtime-cjs-dev': {
    entry: resolve('web/entry-runtime.js'),
    dest: resolve('dist/vue.runtime.common.dev.js'),
    format: 'cjs',
    env: 'development',
    banner
  },
```

runtime/index.js可以看到 Vue是 从‘core/index’中引用的，找到了Vue的入口

```js
import Vue from 'core/index'
```



### 1.2 初始化（core/index.js）

#### 1.2.1 初始化vue

进行了一些基本信息的初始化

```javascript
initMixin(Vue)
stateMixin(Vue)
eventsMixin(Vue)
lifecycleMixin(Vue)
renderMixin(Vue)
```

##### 1.2.1.1 initMixin  初始化 init

```js
// 初始化生命周期
initLifecycle(vm)
// 监听
initEvents(vm)
// 定义reader所需要的一些函数，定义vm的方法
initRender(vm)
callHook(vm, 'beforeCreate')
// 递归查找父级中是否有对应的 provide
initInjections(vm) // resolve injections before data/props
// 初始化 data/props
initState(vm)
// 定义vm._provided = typeof provide === 'function' ? provide.call(vm) : provide
initProvide(vm) // resolve provide after data/props
callHook(vm, 'created')
```

##### 1.2.1.2 stateMixin(Vue)

Vue原型链挂载$data, $props, $set, $delete,$watch

- $set, $delete 触发双向绑定，ob.dep.notify() 进行视图更新，可以用来解决，vue数组，对象数据变化，无法触发视图更新的问题

- $watch  内部新建了一个 watcher 

  ```
  if (isPlainObject(cb)) {
  	return createWatcher(vm, expOrFn, cb, options)
  }
  options = options || {}
  options.user = true
  const watcher = new Watcher(vm, expOrFn, cb, options)
  if (options.immediate) {
      try {
          cb.call(vm, watcher.value)
      } catch (error) {
          handleError(error, vm, `callback for immediate watcher "${watcher.expression}"`)
  	}
  }
  return function unwatchFn () {
  	watcher.teardown()
  }
  ```

##### 1.2.1.3  eventsMixin(Vue)

定义了 $on, $once, $off, $emit

##### 1.2.1.4 lifecycleMixin(Vue)

完善生命周期函数，定义了_update, $forceUpdate, $destroy

##### 1.2.1.5 renderMixin(Vue)

挂载了 $nextTick, _render

- $nextTick 内部返回了一个Promise
- _render返回 VNode


#### 1.2.2 进行了很多Vue相关Api的初始化

global-api/index

```js
// 初始化vue组件的安装 需要 plugin.install 为 function 或者 plugin本身 为 function
initUse(Vue) 
// 将options merge 到 Vue中
initMixin(Vue)
initExtend(Vue)
initAssetRegisters(Vue)
```

