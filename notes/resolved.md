Q: el-table-column 用 v-for 生成，改变数组后 `<template slot="header">` 中试图不能触发更新

A: 清空原数组，在 nextTick 中重新赋值

Q: import 与 require 有哪些区别

A:
1. require/exports 是 CommonJS 规范引入方式；import/export 是 es6 的一个语法标准

2. require/exports 是运行时动态加载，import/export 是静态编译

3. require/exports 输出的是一个值的拷贝，import/export 模块输出的是值的引用

> CommonJS 加载的是一个对象（即 module.exports 属性），该对象只有在脚本运行完才会生成。而 ES6 模块不是对象，它的对外接口只是一种静态定义，在代码静态解析阶段就会生成。- 阮一峰

> https://zhuanlan.zhihu.com/p/121770261