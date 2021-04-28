# 实例模板

+ 提供标签用域填充数据
+ 引入vue.js
+ 使用vue的语法做功能
+ 把vue提供的数据填充到标签里
```
<body>
	<div id="app">
		<div>{{msg + '---' +123}}</div>
        <!-->插值表达式，将数据填充到HTML标签中，支持简单的计算<-->
	</div>
	<script src="vue.js"></script>
	<script>
		var vm = new Vue({
			el:'#app',
            // 元素的挂载位置（值可以是css选择器或者DOM元素）
			data:{
                // 模型数据 是一个对象
				msg:'hello vue'
			}
		})
	</script>
</body>
```
#  模板语法
1. 差值表达式
2. 指令（自定义属性 v-）
    ```
    var vm = new Vue({
        el:'#app',
        data:{
            msg:'hello vue',
            msg1:'<h1>hello vue</h1>',
        }
    })
    ======================================
    v-cloak
+ 解决初始化的时候页面闪动，会出现模板语法的问题
    [v-cloak]{
        display: none;
    }
    <div v-cloak>{{msg}}</div>
    -------------------------------------
+ v-text
    // 填充纯文本,不灵活，一般不用
    <div v-text="msg"></div>
    -------------------------------------
 +   v-html
    // 填充HTML片段
        // 存在安全问题
        // 内部数据可用，跨域数据不可用
    <div v-html="msg1"></div>
    -------------------------------------
  +  v-pre
    // 填充原始信息,跳过编译过程
    <div v-pre>{{msg}}</div>
    // 显示{{msg}}
    --------------------------------------
   + v-once
    // 显示的信息后期不需要修改,可以添加v-once,调高性能
    // 数据响应式:数据改变会导致页面内容跟着改变
    <div v-once>{{msg}}</div>
    ---------------------------------------
 + v-model
    // 双向数据绑定(数据绑定,DOM监听)
    <div>{{ msg }}</div>
    <input type="text" v-model='msg'>
```

+  v-model相当于两个方法的综合
 ```
    <input type="text" :value="msg" :input="valueChange">
    <h2>{{msg}}</h2>
    <script>
        data: {msg: 'hello'},
        methods:{
            valueChange(event){
                this.msg = event.target.value
            }
        }
    </script>

# 事件绑定v-on

```
var vm = new Vue({
    el:'#app',
    data:{
        num : 1
    },
    methods:{
    	handle1: function(event) {
    		console.log(event.target.innerHTML)
    		this.num++
    	},
    	handle2: function(p,event) {
    		console.log(event.target.innerHTML)
    		console.log(p)
    		this.num++
    	}
    }
})

v-on
<button id="app" v-on:click='num++'>{{ num }}</button>
            简写
<button id="app" @click='num++'>{{ num }}</button>

<button id="app" @click='handle'>{{ num }}</button>
如果事件直接绑定函数名称，那么默认会传递事件对象作为事件函数的第一个参数
<button id="app" @click='handle2(p, $event)'>{{ num }}</button>
如果事件绑定函数调用，那么事件对象必须作为最后一个参数显示传递，并且事件对象的名称必须是$event
```
