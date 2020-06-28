[TOC]

### 1. Vue cli

#### 1.1 runtimeCompiler和runtimeOnly

- runtimeCompiler 使用注册组件形式使用App
  - template -> ast(抽象语法树(abstract syntax tree)) -> render -> vDom (virtual dom) -> 真实dom
- runtimeOnly 使用render函数渲染App 
  - render -> vDom (virtual dom) -> 真实dom

- runtimeOnly 性能更好

### 2. Vuex

#### 2.1 Action和Mutation

- 可以跳过Action，直接使用 mutation 直接操作 state

- 异步操作必须通过Action去修改state 否则 Vue devtools无法跟踪状态变化

  

### 3. vue

#### 3.1 数据更新，视图不更新

```javascript
https://blog.csdn.net/weixin_41767649/article/details/82797373
可以使用 vue.$set('key',value)
```

#### 3.2  vue Filters中不支持 this 建议使用computed，methods

------

### yarn

#### 1. yarn install报错 retrying

```shell
yarn install 默认超时时间为30s 修改超时时间即可
yarn install --network-timeout 100000
```

------

#### 

