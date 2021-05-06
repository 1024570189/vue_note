# 侦听器  当数据发生变化时，及时做出响应处理。
## watch 侦听  一旦数据变化就会触发侦听器绑定的方法  侦听的是 data 数据 

```

    var vm = new Vue({
    el: '#demo',
    data: {
        firstName: 'Foo',
        lastName: 'Bar',
        fullName: 'Foo Bar'
    },
    watch: {  侦听 data中的 firstName 属性firstName 一旦发生变化 就会 触发这个方法
        firstName: function (val) {
            this.fullName = val + ' ' + this.lastName
        },
        lastName: function (val) {
            this.fullName = this.firstName + ' ' + val
        }
    }
    })
```
+ 案例

```
    <body>
        <div id="app">
            <input type="text" v-model="message">
            <h2>状态：{{ state }}</h2>
        </div>
    </body>

    let vm = new Vue({
        el: '#app',
        data: {
            message: 'Hello',
            state: '【未修改】'
        },
        
        watch: {//监听属性
        
            // 绑定监听数据
            message: function (e){
            // 当数据修改时做出响应(处理函数)  当 inpur输入框里面的 message 改变时，message 是双向绑定的，改变了就会触发到这个方法，同时state就会变成 已修改。
            this.state = '【已修改😀】'
            // ...
            }
        }
    })
```
## vue 侦听器 watch 之 深度监听(deep)的用法

### 监听一个对象整体的变化 就是监听对象所有属性值的变化
```
<template>
  <div>
   监听obj中data1属性的变化:
   <input type="text" v-model="obj.data1">
   <br>
    监听obj中data2属性的变化:
   <input type="text" v-model="obj.data2">
  </div>
</template>

data () {
    return {
      obj:{
        data1:'我是data1的初始值',
        data2:'我是data2的初始值'
      }
    }
  },
  watch:{
    obj:{
      handler:function(newvalue){
        console.log('新值：');
        console.log(newvalue);
      },
      deep:true    //deep置为true表示：对象中任一属性值发生变化，都会触发handler中的方法
      如果不写 deep true的话 是侦听不到对象里面的内层属性的变化的。
    }
  },

```



# 生命周期
 ### 主要阶段
 + 挂载
    - beforeCreate
    - created
+ 挂载
    - beforeMount
    - mounted
+ 更新
    - beforeUpdate
    - updated
+ 销毁
    - beforeDestroy
    -  destroyed


    #### Vue实例的产生过程
    + beforeCreate在实例初始化之后，数据观测和事件配置之前被调用。

    + created在实例创建完成后被立即调用。  可以在进入页面的时候 把初始化数据的接口方法 放在这里运行一下。

    + beforeMount在挂载开始之前被调用。

    +  mounted el被新的建的vm.$el替换，并挂载到实例上去之后调用该钩子。

    + beforeUpdate数据更新时调用，发生在虚拟DOM打补丁之前。

    + updated由于数据更改导致的虚拟DOM重新渲染和打补丁，在这之后会调用该钩子。

    + beforeDestroy实例销毁之前调用。

    + destroyed实例销毁后调用。
