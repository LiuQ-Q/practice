# vue

Q: el-table-column 用 v-for 生成，改变数组后 `<template slot="header">` 中试图不能触发更新

A: 清空原数组，在 nextTick 中重新赋值

Q: v-if 与 v-show 的区别

A:
1. 实现方式: v-if 是通过控制 DOM 节点的存在与否来控制元素的显隐; v-show 是通过设置 DOM 元素的 display 样式，block 为显示，none 为隐藏

2. 编译过程: v-if 切换有一个局部编译/卸载的过程，切换过程中合适地销毁和重建内部的事件监听和子组件; v-show 只是简单的基于 css 切换

3. 编译条件: v-if 是惰性的，如果初始条件为假，则什么也不做; 只有在条件第一次变为真时才开始局部编译; v-show 是在任何条件下（首次条件是否为真）都被编译，然后被缓存，而且 DOM 元素保留

4. 性能消耗: v-if 有更高的切换消耗; v-show 有更高的初始渲染消耗

Q: vue 拦截器

A:
1. 路由拦截器

```
// to 代表要去的路由指向, from 指从哪个路由跳转过来的, next 为判断操作, 如果为空表示放行
router.beforeEach((to, from, next) => {
  if (localStorage.getItem('token) || to.name === 'login) {
    next()
  } else {
    next({ path: '/ })
  }

  next()
})
```

Q: 权限控制

A:
1. 页面级权限控制

通常通过模块的可见性控制权限（例：财务人员只能看财务模块）

2. 功能级权限控制

根据功能划分权限（例：给张三赋予“人力资源总管”角色，“人力资源总管”具有“修改员工”、“删除员工”的权限）

3. 数据级权限控制

根据用户可查看的数据划分权限（例：张三可查看自己部门的数据，也可以设置张三查看其它部门数据）

vue 实现
1. 前端将公共部分的路由直接写在前端代码中，其余部分前端开发完成后，将对应路由复制一份给后端根据权限匹配存储在数据库中，用户登录时，从后端数据库中根据用户权限读取对应路由返回给前端，再添加到前端路由中动态渲染上。
这样操作，需要前后端密切配合，而且页面中的操作级权限不能控制；

2. 在路由的meta属性中增加 roles字段，用来存储可访问当前路由的权限，在路由全局守卫router.beforeEach中进行拦截处理，示例如下：

```
// 路由信息
routes: [
    {
        path: '/login',
        name: 'login',
        meta: {
            roles: ['admin', 'user']
        },
        component: () => import('../components/Login.vue')
    },
    {
        path: 'home',
        name: 'home',
        meta: {
            roles: ['admin']
        },
        component: () => import('../views/Home.vue')
    },
]
```

```
// 页面控制
// 假设角色有两种：admin 和 user
// 这里是从后台获取的用户角色
const role = 'user'
// 在进入一个页面前会触发 router.beforeEach 事件
router.beforeEach((to, from, next) => {
    if (to.meta.roles.includes(role)) {
        next()
    } else {
        next({path: '/404'})
    }
})
```

3. 通过自定义指令实现 操作级权限控制

[vue官网-自定义指令](https://cn.vuejs.org/v2/guide/custom-directive.html)

示例

```
// 获取用户角色, 接口返回或者cookie中获取
function getRole() {
  return 'admin'
}
 
// 注册一个全局自定义指令 `v-role`
Vue.directive('role', {
  inserted: (el, binding, vnode) => {
    // 从自定义指令中获取当前指令允许的权限
    let roles = binding.value;
 
    // 获取当前用户权限
    let role = getRole();
 
    // 如果没有权限就移除此节点
    if (!roles.includes(role)) {
      el.parentNode.removeChild(el);
    }
  }
})
```

```
<template>
  <div>
    <div v-role="['user', 'admin', 'superAdmin']">user</div>
    <div v-role="['admin', 'superAdmin']">admin</div>
    <div v-role="['superAdmin']">superAdmin</div>
  </div>
</template>
```

# js

Q: import 与 require 有哪些区别

A:
1. require/exports 是 CommonJS 规范引入方式; import/export 是 es6 的一个语法标准

2. require/exports 是运行时动态加载，import/export 是静态编译

3. require/exports 输出的是一个值的拷贝，import/export 模块输出的是值的引用

> CommonJS 加载的是一个对象（即 module.exports 属性），该对象只有在脚本运行完才会生成。而 ES6 模块不是对象，它的对外接口只是一种静态定义，在代码静态解析阶段就会生成。- 阮一峰

> https://zhuanlan.zhihu.com/p/121770261

# webpack

Q: loader 作用，和 plugin 有什么区别

A: 
1. loader 可以将文件从不同的语言（TS 或者 ES6）转换为 JS，或者将内联图像转换为 data URL，它是一个转换器，将 A 文件进行编译形成 B 文件，这里操作的是文件，比如将 A.scss 转换为 A.css，单纯的文件转换过程

2. 在 webpack 运行的生命周期中会广播出许多事件，plugin 可以监听这些事件，在合适的时机通过 webpack 提供的 API 改变输出结果。plugin 是一个扩展器，它丰富了 webpack 本身，针对是 loader 结束后，webpack 打包的整个过程，它并不直接操作文件，而是基于事件机制工作，会监听 webpack 打包过程中的某些节点，执行广泛的任务。

