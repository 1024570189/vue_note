# 基于Vue的表单操作
+ input 单行文本
```
    <input type="text" v-model='uname'>
        data: {
            uname: 'jack'
        }
        双向绑定
```

+ submit
```
<input type="sublime" @click.prevent='handle'>
        methods: {
            handle: function() {
                // ajax
            }
        }
        双向绑定

```
+ textarea 多行文本
```
    <textarea v-model="text"></textarea>
    // Vue里面 内容不能写在标签里面
        data: {
            text: "hello world"
        }
```
+ select 下拉多选
```
    单选
    <select  v-model="hobby">
        <option value="1">唱</option>
        <option value="2">跳</option>
        <option value="3">rap</option>
    </select>

    data: {
        hobby: 1
    }
    =============================================
    多选(shift)
    <select  v-model="hobby" multiple="true">
    data: {
        hobby: ['2','4']
    }  
```
+ radio 单选
```
    <input type="radio" id='male' value="1" v-model='gender'>
    <label for='male'>男</label>
    <input type="radio" id='female' value="2" v-model='gender'>
    <label for='female'>女</label>
    data: {
        gender: 1
    // 默认选中男
    }

```

+ checkbox 多选

```
    <input type="checkbox" id="sing" value="1" v-model="hobby">
    <label for="sing">唱</label>
    <input type="checkbox" id="dance" value="2" v-model="hobby">
    <label for="dance">跳</label>
    <input type="checkbox" id="rap" value="3" v-model="hobby">
    <label for="rap">rap</label>
    <input type="checkbox" id="ball" value="4" v-model="hobby">
    <label for="ball">篮球</label>
    data: {
        hobby: ['2','4']
    }
```

+ 表单域修饰符
```
    number:转化为数值
    trim:去掉开始和结尾的空格
    lazy: 将input事件切换为change事件
    <input type="text" v-model.number='age'>
    // 转换为数值
    <input type="text" v-model.trim='info'>
    // 中间的空格去不掉  去除两边的空格
    <input type="text" v-model.lazy='msg'>
    // 不加msg只要改变div内容就变 加了lazy失去焦点才变
    <div>{{msg}}</div>
    data: {
        age: '',
        info: '',
        msg:''
    }
```