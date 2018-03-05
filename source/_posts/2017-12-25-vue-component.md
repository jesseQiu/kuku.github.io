---
title: Vue Component
date: 2017-12-21 14:34:39
updated: 2017-12-21 14:34:39
categories: Vue.js
---
## 1. 定制组件 v-model
> 在一个组件中，v-model 默认使用 value 作为 prop，以及默认使用 input 作为监听事件，但是一些输入框类型，例如 checkbox 和 radio，可能会用到 value。在这种情况下，为了避免冲突，就会需要使用组件的 model 选项：
```js
Vue.component('my-checkbox', {
  model: {
    prop: 'checked',
    event: 'change'
  },
  props: {
    checked: Boolean,
    // 这样可以将 `value` prop 释放出来，用作其他用途
    value: String
  },
  // ...
})
<my-checkbox v-model="foo" value="some value"></my-checkbox>
以上等同于：
<my-checkbox
  :checked="foo"
  @change="val => { foo = val }"
  value="some value">
</my-checkbox>
```
注意，仍然需要显式声明 checked prop 属性。

测试代码:
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>Study Vue</title>
    <script src="./vue.js"></script>
    <style type="text/css">
        #app div{
            margin: 10px;
            border: 1px solid gray;
        }
    </style>
</head>

<body>
    <div id="app">
        <div>
            <!-- 设置 value 值 -->
            <label for="men">Men:</label>
            <input type="radio" id="men" name="sex" value="men" v-model="radioValue">
            <label for="girl">Gril:</label>
            <input type="radio" id="girl" name="sex" value="girl" v-model="radioValue">
            <br> radioValue: {{radioValue}}
        </div>

        <div>
            <!-- 使用默认值 -->
            <input type="checkbox" id="red" v-model="checkboxRed">
            <label for="red">Red</label>
            <input type="checkbox" id="blue" v-model="checkboxBlue">
            <label for="blue">Blue</label>
            <br> checkboxRed: {{checkboxRed}}  checkboxBlue: {{checkboxBlue}}
        </div>

        <div>
            <!-- 设置 checkbox 值 -->
            <input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
            <label for="jack">Jack</label>
            <input type="checkbox" id="john" value="John" v-model="checkedNames">
            <label for="john">John</label>
            <input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
            <label for="mike">Mike</label>
            <br>
            <span>Checked names: {{ checkedNames }}</span>
        </div>

        <hr>
        <h3>正常的 input 组件</h3>
        <input-component v-model="inputModel"> </input-component>
        inputModel: {{inputModel}}           

        <h3>不使用 model 属性的 checkbox</h3>
        <checkbox-component-one v-model="childCheckedOne" value="ChildRed"> </checkbox-component-one>
        childCheckedOne: {{childCheckedOne}}        

        <h3>使用了 model 属性的 checkbox,这样 value 可以单独传值</h3>
        <checkbox-component-two v-model="childCheckedTwo" value="ChildBlue"> </checkbox-component-two>
        childCheckedTwo: {{childCheckedTwo}}

    </div>
    <script>
    let app = new Vue({
        el: '#app',
        components: {
            'input-component': {
                props: {
                    value: String
                },
                template:`<div>
                    <input type="text" id="childGreen" :value="value" @input="input">
                    <label for="childGreen">Green</label>                    
                    <br> value: {{value}}
                </div>`,
                methods:{
                    input(event){
                        console.log('input()',event.target.checked,event.target.value);
                        this.$emit('input',event.target.value)
                    }
                }
            },               
            'checkbox-component-one': {
                props: ['value'],
                template:`<div>
                    <input type="checkbox" id="childRed" @change="change">
                    <label for="childRed">Red</label>                    
                    <br> value: {{value}}
                </div>`,
                methods:{
                    change(event){
                        console.log('change()',event.target.checked,event.target.value);
                        this.$emit('change',event.target.value)
                    }
                }
            },            
            'checkbox-component-two': {
                model: {
                    prop: 'checked',
                    event: 'change'
                },
                props: {
                    checked: Boolean,
                    // 这样可以将 `value` prop 释放出来，用作其他用途
                    value: String
                },
                template:`<div>
                    <input type="checkbox" id="childBlue" :checked="checked" :value="value" @change="change">
                    <label for="childBlue">Blue</label>                    

                    <br> checked: {{checked}} value: {{value}}
                </div>`,
                methods:{
                    change(event){
                        console.log('change()',event.target.checked,event.target.value);
                        this.$emit('change',event.target.checked ? true:false)
                    }
                }
            }
        },
        data: {
            radioValue: '',
            checkboxRed:false,
            checkboxBlue:true,
            checkedNames: [],
            inputModel:'hello input',
            childCheckedOne:true,
            childCheckedTwo:true
        }
    })
    </script>
</body>

</html>
```

## 二、作用于插槽
>作用域插槽是一种特殊类型的插槽，用于将要渲染的元素(already-rendered-elements)，替换为一个（能够传递数据的）可重用模板。
原理其实就是，把子组件中的定义的 `text` 属性数据拿出来，在父组件作用域里重新组织显示。
主要的应用场景是，可以根据实际需求，定制化显示模板的内容。(默认和根据父组件传递的值显示)
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>Study Vue</title>
    <script src="./vue.js"></script>
    <style type="text/css">
    #app div {
        margin: 10px;
        border: 1px solid gray;
    }
    </style>
</head>

<body>
    <div id="app">
        <child-component-one>
            <template slot-scope="props">
                props： {{props}}
                <div>people info: </div>
                <h1>{{ props.text }}</h1>
                <h5>{{ props.text }}</h5>
            </template>
        </child-component-one>   

        <hr>

        <child-component-two>
            <template slot-scope="props">
                props： {{props}}
                <h1>{{ props.text }}</h1>
                <!-- <li>{{ props.text }}</li> -->
            </template>
        </child-component-two>
    </div>
    <script>
    let app = new Vue({
        el: '#app',
        components: {
            'child-component-one': {
                template: `<div>
                    <slot text="child name"></slot>
                    <slot text="child age"></slot>
                </div>`
            },
            'child-component-two': {
                template: `<ul>
                    <slot v-for="item in fruit" :text="item.name">{{item.name}}</slot>
                </ul>`,
                data: function() {
                    return {
                        fruit: [
                            { name: '苹果' },
                            { name: '香蕉' },
                            { name: '橙子' }
                        ]
                    }
                },
            }
        },
        data: {
        }
    })
    </script>
</body>

</html>
```

## 三、动态组件
>可以让多个组件使用同一个挂载点，然后动态地在它们之间切换。要实现此效果，我们可以使用 Vue 保留的 `<component>` 元素，将多个组件动态地绑定到 `<component>` 元素的 `is` 属性上
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>Study Vue</title>
    <script src="./vue.js"></script>
</head>

<body>
    <div id="app">
        <component v-bind:is="currentView">
            <!-- 组件在 vm.currentview 变化时改变！ -->
        </component>
    </div>
    <script>
    let app = new Vue({
        el: '#app',
        components: {
            'home': {
                template: `<h1>Home</h1>`
            },
            'index': {
                template: `<h1>Index</h1>`
            },
            'about': {
                template: `<h1>About</h1>`
            }
        },
        mounted(){
            setInterval(val=>{
                this.views[++this.currentIndex] || (this.currentIndex = 0);
                this.currentView = this.views[this.currentIndex]
            },1000)
        },
        data:{
            views:['home','index','about'],
            currentIndex:0,
            currentView:'home'
        }
    })
    </script>
</body>

</html>
```

## 四、异步组件
### 1. 基础
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>Study Vue</title>
    <script src="./vue.js"></script>
</head>

<body>
    <div id="app">
        <component v-bind:is="currentView">
            <!-- 组件在 vm.currentview 变化时改变！ -->
        </component>
    </div>
    <script>
    let app = new Vue({
        el: '#app',
        components: {
            'home': (resolve,reject)=>{
                setTimeout(()=>{
                    resolve({
                        template: `<h1>Home</h1>`
                    })
                },3000)
            },
            'index': {
                        template: `<h1>Index</h1>`
                    },
            'about': {
                template: `<h1>About</h1>`
            }
        },
        mounted(){
            setInterval(val=>{
                this.views[++this.currentIndex] || (this.currentIndex = 0);
                this.currentView = this.views[this.currentIndex]
            },1000)
        },
        data:{
            views:['home','index','about'],
            currentIndex:0,
            currentView:'index'
        }
    })
    </script>
</body>
</html>
```
### 2. 高级
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>Study Vue</title>
    <script src="./vue.js"></script>
</head>

<body>
    <div id="app">
        <component v-bind:is="currentView">
            <!-- 组件在 vm.currentview 变化时改变！ -->
        </component>
    </div>
    <script>
    const AsyncComp = (resolve,reject) => ({
      // 需要加载的组件. 应当是一个 Promise
      component: 
        new Promise((resolve,reject)=>{
            setTimeout(()=>{
                resolve({
                    template: `<h1>Index</h1>`
                })
            },2000)
        }),
      // loading 时应当渲染的组件
      loading: {
                template: `<h1>loading...</h1>`
            },
      // 出错时渲染的组件
      error: {
                template: `<h1>error</h1>`
            },
      // 渲染 loading 组件前的等待时间。默认：200ms.
      delay: 200,
      // 最长等待时间。超出此时间
      // 则渲染 error 组件。默认：Infinity
      timeout: 3000
    });
    let app = new Vue({
        el: '#app',
        components: {
          'index': AsyncComp,
        },
        data:{
            currentView:'index'
        }
    })
    </script>
</body>

</html>
```
## 参考:
- [Vue](https://vuefe.cn/v2/guide/components.html#非父子组件通信-Non-Parent-Child-Communication)
