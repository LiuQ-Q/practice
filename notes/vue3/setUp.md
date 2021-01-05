### setUp 函数

1. setup函数是处于 生命周期函数 beforeCreate 和 Created 两个钩子函数之间的函数 也就说在 setup函数中是无法 使用 data 和 methods 中的数据和方法的
2. setup函数是 Composition API（组合API）的入口
3. 在setup函数中定义的变量和方法最后都是需要 return 出去的 不然无法再模板中使用
4. 由于我们不能在 setup函数中使用 data 和 methods，所以 Vue 为了避免我们错误的使用，直接将 setup函数中的this修改成了 undefined
5. setup函数只能是同步的不能是异步的

```
<template>
  <div id="app">
    {{name}}
    <p>{{age}}</p>
    <button @click="plusOne()">+</button>
  </div>
</template>

<script>
import {ref} from 'vue'
export default {
  name:'app',
  data(){
    return {
      name:'xiaosan'
    }
  },
  setup(){
    const name =ref('小四')
    const age=ref(18)
    function plusOne(){
      age.value++ //想改变值或获取值 必须.value
    }
    return { //必须返回 模板中才能使用
      name,age,plusOne
    }
  }
}
</script>
```