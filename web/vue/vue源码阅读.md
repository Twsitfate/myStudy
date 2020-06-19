[TOC]



## Vue源码阅读笔记

### 1. 入口

package.json中 可看到 build为scripts/build.js

#### 1.1 构建（build.js）

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



#### 1.2初始化（core/index.js）

进行了很多Vue相关Api的初始化

