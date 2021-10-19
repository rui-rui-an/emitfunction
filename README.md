# 子组件emit有参数，父组件也有参数

最近在写业务组件的时候，碰到了一个情况就是当子组件去emit一个事件的时候，子组件传参了，但是在父组件的触发事件上，父组件同时也想去传递一个参数，当时就蒙蔽了，经过多次的尝试，发现$event可以解决这个问题。然后经过了不断地调整关键词搜索，发现了这种情况去可以分为两种情况的。

总结版：

1.子组件传递一个参数，父组件同时加入参数

```js
// 子组件
this.$emit('test',this.param)
// 父组件
@test='test($event,userDefined)'
```

2.子组件传递多个参数，父组件同时加入参数

```js
// 子组件
this.$emit('test',this.param1，this.param2, this.param3)
// 父组件 arguments 是以数组的形式传入
@test='test(arguments,userDefined)'
```

代码版本：

子组件：

```js
<template>
  <div class="son">
    <div class="border">
      <div>我是子组件的内容</div>
      <button @click="btnClick">子组件传递一个参数,父组件自定义参数触发事件</button>
      <button @click="moreValueClick">子组件传递多个参数,父组件自定义参数触发事件</button>
      <button @click="fatherNoValueClick">子组件传递多个参数，父组件无参数触发事件</button>
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    btnClick(){
      this.$emit('btnClick','子组件的参数')
    },
    moreValueClick(){
      this.$emit('moreValueClick','子组件参数1','子组件参数2','子组件参数3')
    },
    fatherNoValueClick(){
      this.$emit('fatherNoValueClick','参数1','参数2','参数3')
    },
  },
}
</script>

<style lang="less" scoped>
.son{
  .border{
    button{
      display: block;
    }
  }
}
</style>
```

父组件：

```js
<template>
  <div class="home">
    <son
      @btnClick="btnClick($event, '父组件参数aaaa')"
      @moreValueClick="moreValueClick(arguments, '父组件自己的参数')"
      @fatherNoValueClick="fatherNoValueClick"
    ></son>
  </div>
</template>

<script>
import son from '@/components/son.vue'
export default {
  name: 'Home',
  components: {
    son
  },
  methods: {
    btnClick (item, b) {
      console.log(item)
      console.log(b)
    },
    moreValueClick (vals, item) {
      console.log(vals)
      console.log(item)
    },
    fatherNoValueClick(a,b,c){
      console.log(a);
      console.log(b);
      console.log(c);
    },
  }
}
</script>

```

参考文章地址：https://www.136.la/tech/show-1122632.html

代码地址：https://github.com/rui-rui-an/emitfunction
