# 快速开始

> [快速上手 | Vue.js (vuejs.org)](https://cn.vuejs.org/guide/quick-start.html)

> 去node下载对于版本的**node**
>
> 打开**命令行**, 切换到想要创建项目的文件夹
>
> 键入命令**`npm create vue@latest`**
>
> 项目创建成功, 接下来可以用安装了volar拓展的**VScode**打开项目目录 
>
> 编写项目时, 可以用命令行在该目录下执行命令**`npm run dev`**来运行项目



# 基础

## 模板语法



+ **v-html**="*varName*"

+ **v-bind**:*argument*="*varName*"       // `v-bind:`可以简写成`:`    当值为object*(包含键值对)*时可以省略*`argument`*

+ **v-on**:*event*="*function*"               // `v-on`可以简写成`@`

    

    > // **指令参数可以动态编写**, 用中括号标注
    >
    > 如下(`@[event]="increment"`中的`event`值为`click`, `increment`为函数名) : 
    >
    > ```vue
    > <template>
    > 	<button @[event]="increment">
    >     	{{ count }}
    >     </button>
    > </template>
    > 
    > <script setup>
    >     import { ref } from 'vue'
    > 
    >     const event = ref('click')
    >     const count = ref(0)
    > 
    >     function increment() {
    >       count.value++
    >     }
    > </script>
    > ```
    > 在动态参数 `[argument]`中, `argument`必须不含空 不含引号 且 **不含大写字母** *(大写字母会被自动转小写导致编译出错)*



![image-20231025155411553](./image-20231025155411553.png)



## 响应式基础

### ref

```vue
<script setup>
    import { ref } from 'vue'

    const count = ref(0)

    function increment() {
      count.value++
    }
</script>

<template>
	<button @click="increment">
    	{{ count }}
    </button>
</template>
```

**ref()** 返回一个包含`value`属性的ref对象, 

当该对象在 template 中时会自动解包, 不需要使用`.value`  *// 如 line 13*

该对象在 scrpt 中需要使用`.value`来访问其值    *// 如 line 7*



### reactive

```vue
<script setup>
	import { reactive } from 'vue'

	const state = reactive({ count: 0 })
</script>

<template>
	<button @click="state.count++">
      	{{ state.count }}
    </button>
</template>
```

**reactive() **返回响应式代理对象, 可以直接访问对象的参数值   *// line 8*

**局限性**:

+ 不能对基本类型进行代理 (如 boolean string number)
+ 不能替换整个对象, 否则被替换的对象失去代理连接
+ 对解构操作不友好

```vue
<script setup>
    import { reactive } from 'vue'

    const state = reactive({ count: 0 })

    // 当解构时，count 已经与 state.count 断开连接
    let { count } = state
    // 不会影响原始的 state
    count++
</script>
```







## 计算属性

```vue
<script setup>
    import { reactive, computed } from 'vue'

    const author = reactive({
      // ...
      books: [
        // .......
      ]
    })

    // 一个计算属性 ref
    const Message = computed(() => {
      return author.books.length > 0 ? 'Yes' : 'No'
    })
</script>
```

> **computed**(() => {
>       return author.books.length > 0 ? 'Yes' : 'No'
> })  
>
> computed内包含的是**箭头函数**
>
> line 13 中 `author.books`是响应式对象, 一旦修改, 就会被重新计算
>
> **非响应式对象在计算属性中不会被重复计算**

+ 可写入计算属性 ....



## 样式绑定

```vue
<template>
	<div :class="{ active: isActive }"></div>
</template>
```

> `active`是一个变量, 表示class的值 , 
>
> `isActive`是一个**布尔值变量**, 表示`active`**是否生效**

<img src="./image-20231025162235092.png" alt="image-20231025162235092" style="zoom:60%;" />

+ 字典 (如上)
+ 对象 (如上)
+ 数组
+ 数组套三元表达式
+ 数组套字典



## 条件渲染

+ v-if   v-else-if   v-else      // 可以在不可见的`Template`包装器标签上加上改指令

<img src="./image-20231025162516888.png" alt="image-20231025162516888" style="zoom:60%;" />

+ v-show  // ......



## 列表渲染

`v-for="item in items"`

`v-for="(item, index) in item"`

`v-for="(key, value, index) for item"`

> for 和 in 等价

+ 解构   //....



## 事件处理

<img src="./image-20231025165228012.png" alt="image-20231025165228012" style="zoom:90%;" />

### 事件修饰符

+ 普通修饰符
+ 键盘按键修饰符 
    + exact修饰符
+ 鼠标按键修饰符



## 表单输入绑定 (v-model)

将表单输入内容与相对应的变量同步

<img src="./image-20231025171316566.png" alt="image-20231025171316566" style="zoom:75%;" />

修饰符: lazy  number  trim



## 生命周期

[组合式 API：生命周期钩子 | Vue.js (vuejs.org)](https://cn.vuejs.org/api/composition-api-lifecycle.html)



## 侦听器

### watch(source, callback, dict=null)

**source**

+ 可以是被侦听的变量     
    + x  y  name  cnt   age
+ 可以是被侦听的变量属性或表达式(用**箭头函数**)     
    + ()=>x.value    ()=>y.age     ()=>x.value+y.value
+ 也可以是<u>多个</u>被侦听的变量(用**数组**)
    + [x, y]    [x, ()=>y.cnt ]      [()=>x.value,  ()=>a.val+b.val ]

**callback**

+ 可以是一个参数 (新值)
    + (newValue) => { ...**`${newValue}`**... }
+ 也可以是两个参数  (新值, 旧值)
    + (newValue, oldValue) => {....**`${newValue}`**... **`${oldValue}`**...}
+ 参数也可以是数组
    + ([newX, newY], [oldX, oldY]) => {....**`${newX}`**... **`${oldY}`**...}


**dict**

>  默认可以不写该参数, 以`{argu01: val01, argu02: val02, ...}`的形式传参

+ { deep: ture}  // 强制转化为深层侦听器
+ { immediate: true }  // 侦听器被创建之时**立刻执行一次**



### watchEffect(callback, dict=null)

> 与 **watch(source, callback, { immediate: true })** 等价,
>
> 该侦听器创建之后会立刻执行一次, 找出`callback`中的`source`并自动进行跟踪



### watchPostEffect

> 默认情况下, 在响应式依赖更改前会执行callback, 如果想要更改后执行, 可以用 **`{  flush: 'post' }`**
>
> 后置刷新的 **`watchEffect()`** 有个更方便的别名 **`watchPostEffect()`**



### 关闭侦听器

> 同步创建的侦听器会自动关闭
>
> 而**异步创建**的侦听器需要**手动关闭**
>
> ![image-20231026160655628](./image-20231026160655628.png)

## 模板引用 ?



## 组件基础

### 组件的定义与使用

定义  (文件名`ButtonCounter.vue`)

```vue
<script setup>
import { ref } from 'vue'

const count = ref(0)
</script>

<template>
  <button @click="count++">You clicked me {{ count }} times.</button>
</template>
```

使用

```vue
<script setup>
import ButtonCounter from './ButtonCounter.vue'
</script>

<template>
  <h1>Here is a child component!</h1>
  <ButtonCounter />
</template>
```



### Props的定义与使用

定义  (文件名 `BlogPost.vue`)

```vue
<!-- BlogPost.vue -->
<script setup>
defineProps(['title'])
</script>

<template>
  <h4>{{ title }}</h4>
</template>
```

使用

```vue
<BlogPost title="My journey with Vue" />
<BlogPost title="Blogging with Vue" />
<BlogPost title="Why Vue is so fun" />
```



### 监听事件

定义 (文件名 `BlogPost.vue`)

```vue
<!-- BlogPost.vue-->
<script setup>
defineProps(['title'])  //声明传入值
defineEmits(['enlarge-text']) // 声明传出事件
</script>

<template>
  <div class="blog-post">
    <h4>{{ title }}</h4> 
    <button @click="$emit('enlarge-text')">Enlarge text</button>
  </div>
</template>
```



使用  // 局部代码

```vue
<div :style="{ fontSize: postFontSize + 'em' }">
  <BlogPost
    v-for="post in posts"
    :key="post.id"
    :title="post.title"
    @enlarge-text="postFontSize += 0.1"
   />
</div>
```



### 插槽

> 定义组件时, 在`template`中加入占位符**`<slot />`**
>
> 使用组件时, 在组件双标签内插入内容, (内容会插入插槽)



### 动态组件 //...

// ....



# 深入组件

![image-20231026195154497](./image-20231026195154497.png)

## 注册

### 全局注册

```vue
<script setup>
    import MyComponent from './App.vue'
	app.component('MyComponent', MyComponent)
</script>
```

```vue
<script setup>
    //.....
    // 链式调用
    app
      .component('ComponentA', ComponentA)
      .component('ComponentB', ComponentB)
      .component('ComponentC', ComponentC)
</script>
```



### 局部注册

```vue
<script setup>
import ComponentA from './ComponentA.vue'
</script>

<template>
  <ComponentA />
</template>
```



## Props

### 静态传参

![image-20231026195739391](./image-20231026195739391.png)



### 动态传参

```vue
// 将变量name传入组件的title属性中
<script setup>
	//...
    const name = ref('czp')
</script>

<template>
  	<BlogPost title="name" />
</template>
```



### map传参

当有多个参数需要传入的时候, 可以创建一个map ,即 `{key: val, ...}`

将参数名定义为`key`,参数值定义为`val`,然后传入 map 即可

<img src="./image-20231026200253570.png" alt="image-20231026200253570" style="zoom:67%;" />

### Props校验 //...

//.....



## 事件

### 事件参数

![image-20231026200950157](./image-20231026200950157.png)



### 事件校验

<img src="./image-20231026201830854.png" alt="image-20231026201830854" style="zoom:80%;" />



## 透传Attribute //....

//.....



## 依赖注入

> 祖先组件通过`provide()`注册依赖, 后代组件通过`inject()`获取依赖
>
> 这样做的好处是: 避免了依赖在组件间层层透传

### 供给方 (**`provide`**)

```vue
<script setup>
import { provide } from 'vue'

provide(/* 注入名 */ 'message', /* 值 */ 'hello!')
</script>
```

### 注入方 (**`inject`**)

```vue
<script setup>
    import { inject } from 'vue'

    const message = inject('message')
</script>
```

当然, 如果注入了不存在的依赖可以设定默认值, 如下:

```vue
<script setup>
    import { inject } from 'vue'

    // 如果没有祖先组件提供 "message"
    // `value` 会是 "这是默认值"
    const value = inject('message', '这是默认值')
</script>
```

+ **工厂函数**  //...
+ readonly  //..

### 使用 Symbol 作注入名

> Symbol标记一块内存, 不易被覆盖, 比字符串变量名更安全
>
> // 因为 `"getName" == "getName"`    `Symbol() != Symbol()`

<img src="./image-20231027111536627.png" alt="image-20231027111536627" style="zoom: 80%;" />



## ==异步组件==

延时加载

```vue
<script setup>
    import { defineAsyncComponent } from 'vue'

    const AdminPage = defineAsyncComponent(() =>
      import('./components/AdminPageComponent.vue')    // 延时加载
    )
</script>

<template>
  	<AdminPage />
</template>
```



# 逻辑复用

## 组合式函数

> 可嵌套
>
> 一下是Demo

**定义**

```js
// mouse.js
import { ref, onMounted, onUnmounted } from 'vue'

// 按照惯例，组合式函数名以“use”开头
export function useMouse() {
  // 被组合式函数封装和管理的状态
  const x = ref(0)
  const y = ref(0)

  // 组合式函数可以随时更改其状态。
  function update(event) {
    x.value = event.pageX
    y.value = event.pageY
  }

  // 一个组合式函数也可以挂靠在所属组件的生命周期上
  // 来启动和卸载副作用
  onMounted(() => window.addEventListener('mousemove', update))
  onUnmounted(() => window.removeEventListener('mousemove', update))

  // 通过返回值暴露所管理的状态
  return { x, y }
}
```

**使用**

```vue
<script setup>
import { useMouse } from './mouse.js'

const { x, y } = useMouse()
</script>

<template>Mouse position is at: {{ x }}, {{ y }}</template>
```



### ++?



## ==自定义指令==

<img src="./image-20231027164504191.png" alt="image-20231027164504191" style="zoom:67%;" />

### 钩子函数

```js
const myDirective = {
  // 在绑定元素的 attribute 前
  // 或事件监听器应用前调用
  created(el, binding, vnode, prevVnode) {
    // 下面会介绍各个参数的细节
  },
  // 在元素被插入到 DOM 前调用
  beforeMount(el, binding, vnode, prevVnode) {},
  // 在绑定元素的父组件
  // 及他自己的所有子节点都挂载完成后调用
  mounted(el, binding, vnode, prevVnode) {},
  // 绑定元素的父组件更新前调用
  beforeUpdate(el, binding, vnode, prevVnode) {},
  // 在绑定元素的父组件
  // 及他自己的所有子节点都更新后调用
  updated(el, binding, vnode, prevVnode) {},
  // 绑定元素的父组件卸载前调用
  beforeUnmount(el, binding, vnode, prevVnode) {},
  // 绑定元素的父组件卸载后调用
  unmounted(el, binding, vnode, prevVnode) {}
}
```



### 钩子参数 ?

<img src="./image-20231027165502155.png" alt="image-20231027165502155" style="zoom:67%;" />





# ==blog-Vue3==



## 环境准备

<img src="./image-20231204104036613.png" alt="image-20231204104036613" style="zoom:67%;" />







## | Vue

项目创建

> **`npm init vue@latest`**   或   **`npm create vue@latest`**   创建一个项目目录
>
> **`cd ...`**  切换到项目目录下
>
> **`npm install`**  下载vue相关内  
>
> // 这里还可以**下载**一些其他内容  如`npm install axios` `npm install element-plus --save`
>
> // 可以进行**编码** 然后执行下面的命令来运行项目
>
> **`npm run dev`**   运行项目

<img src="./image-20231203150004755.png" alt="image-20231203150004755" style="zoom:67%;" />

项目结构

![image-20231203150527902](./image-20231203150527902.png)

项目启动

![image-20231203150851065](./image-20231203150851065.png)

**<img src="./image-20231203152009261.png" alt="image-20231203152009261" style="zoom: 67%;" />**



单文件组件

![image-20231203160334166](./image-20231203160334166.png)

**组合式**(推荐) 和 选项式![image-20231203160821798](./image-20231203160821798.png)





## | Axios

> 可以在前端向后端发送请求, 完成前后端的连接

需要在项目文件下用终端命令**`npm install axios`**

<img src="./image-20231201182257671.png" alt="image-20231201182257671" style="zoom: 67%;" />

![image-20231201182430684](./image-20231201182430684.png)

> 防止**bug**: **==接受请求的后端控制层需要加上注解`@CrossOrigin`==**
>
> ![image-20231201182949147](./image-20231201182949147.png)
>
> <img src="./image-20231201183507866.png" alt="image-20231201183507866" style="zoom:67%;" />

![image-20231201184226143](./image-20231201184226143.png)







## ==| Element Plus==

> 详情请看官网:  [Element Plus | Vue3组件库 (element-plus.org)](https://element-plus.org/zh-CN/#/zh-CN)

<img src="./image-20231203190721812.png" alt="image-20231203190721812" style="zoom:67%;" />



### 快速入门

<img src="./image-20231203191937620.png" alt="image-20231203191937620" style="zoom:67%;" />



#### npm下包

> 在项目目录下执行命令**`npm install element-plus --save`**



#### main引入组件

> line 2 3 8

```js
import {createApp} from 'vue'
import ElementPlus from 'element-plus'   //导入element-plus
import 'element-plus/dist/index.css'   //导入element-plus的样式
import App from './App.vue'

const app = createApp(App);

app.use(ElementPlus);   //使用element-plus
app.mount('#app');
```





### 常用组件

#### 汉化包

![image-20231204144321150](./image-20231204144321150.png)



#### 按钮



#### 表格



#### 分页条









## | Pinia与token

<img src="./image-20231204122552773.png" alt="image-20231204122552773" style="zoom: 50%;" />

![image-20231204123034612](./image-20231204123034612.png)



### 持久化插件persist

![image-20231204153148170](./image-20231204153148170.png)







## js导入

![image-20231201180425793](./image-20231201180425793.png)

>  <img src="./image-20231201180956619.png" alt="image-20231201180956619" style="zoom:50%;" />
>
>  起别名
>
>  ![image-20231201181606725](./image-20231201181606725.png)
>
>  默认导出
>
>  ![image-20231201181922192](./image-20231201181922192.png)







## ==api相关(await async)==

> 将与异步请求的api相关的函数同一放到一个js文件中

### api定义

![image-20231203183159634](./image-20231203183159634.png)

> 注意: 定义和调用异步请求时都需要注明`await`和`async`以同步等待结果

### api调用

![image-20231203183450490](./image-20231203183450490.png)

> 注意: 第2行文件路径*`@/api/article.js`*, 其中**`@`**表示**`src`**文件夹



## 传参类型 ?

### 请求参数

```js
const p = {
    name: 'czp',
    pwd: '123'
}

request.get('/xxx', {params:{...p}});
```



### Json

```js
const p = {
    name: 'czp',
    pwd: '123'
}

request.get('/xxx', p);
```



### x-www-form-urlencoded

<img src="./image-20231204115856499.png" alt="image-20231204115856499" style="zoom:60%;" />

```js
const p = {
    name: 'czp',
    pwd: '123'
}

const params = new URLSearchParam();
for(let key in p) {
    params.append(key, p[key]);
}
request.get('/xxx', params);
```





## ==拦截器==



### 初始响应拦截器

![image-20231203185433569](./image-20231203185433569.png)

> > 请求前缀优化
> >
> > **`axion.create()`**
>
> ```js
> axios.get('http://localhost:8080/user');
> ```
>
> 与
>
> ```js
> const baseURL = 'http://localhost:8080';
> const instance = axios.create({baseURL});
> 
> instance.get('/user');
> ```
>
> 这连段代码的请求结果是等价的, 但后者的封装了通用url前缀, 减少了代码冗余, 提高了代码的可维护性





### 拦截器的调用 (api定义)

![image-20231203185905195](./image-20231203185905195.png)

> 因为拦截器本身是异步的, 所以api这里的`async`和`await`可以省略 *( line 9 10 14 15)*,
>
> 调用api时还需要使用`async`和`await`







### 响应拦截器

![image-20231204122039565](./image-20231204122039565.png)



<img src="./image-20231204153654432.png" alt="image-20231204153654432" style="zoom:75%;" />

> <img src="./image-20231204160453674.png" alt="image-20231204160453674" style="zoom:67%;" />









### 请求拦截器

<img src="./image-20231204152417092.png" alt="image-20231204152417092" style="zoom:60%;" />









## ==| vue-router==

<img src="./image-20231204155109092.png" alt="image-20231204155109092" style="zoom:50%;" />

![image-20231204155527476](./image-20231204155527476.png)



<img src="./image-20231204160503202.png" alt="image-20231204160503202" style="zoom:67%;" />



### 子路由

![image-20231204163636625](./image-20231204163636625.png)

1. 配置子路由

![image-20231204165901793](./image-20231204165901793.png)

2. 设置index属性![image-20231204164346123](./image-20231204164346123.png)





## 问题解决



### 跨域问题

简单解决: 在对应的后端控制层加上注解**`@CrossOrigin`**

详细解决: [实战篇-61_大事件_跨域解决_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV14z4y1N7pg?p=73&spm_id_from=pageDriver&vd_source=c89b8a0f62dff894bf18ace7e0f4c032)

![image-20231205174014646](./image-20231205174014646.png)

> <img src="./image-20231205174134417.png" alt="image-20231205174134417" style="zoom:67%;" />





### useRouter引入错误

> [Vue warn]: inject() can only be used inside setup() or functional components.

将

```js
import { useRouter } from 'vue-router';
const router = useRouter();
```

换成

```js
import router from '@/router/index.js'
```

> 其中, 路径为`@/router/index.js`文件内容如下;
>
> <img src="./image-20231205185924002.png" alt="image-20231205185924002" style="zoom: 67%;" />







## 示例代码

### 异步请求

![image-20231203163142444](./image-20231203163142444.png)

![image-20231203164303382](./image-20231203164303382.png)

解构

```vue
<script>
    const obj1 = ref({a:1, b:2});
    const obj2 = ref({...boj1.value});   // obj2==obj1
    // a = {name:czp, age:21};
    // b = {...a};   //b==a
</script>
```



### 表单校验

> 详见官网

模型和校验规则的定义

<img src="./image-20231204114926067.png" alt="image-20231204114926067" style="zoom:80%;" />

> 注意: 自定义校验内部需要更改:
>
> <img src="./image-20231204114848144.png" alt="image-20231204114848144" style="zoom:67%;" />



模型和校验规则的绑定

![image-20231204114525174](./image-20231204114525174.png)

 

### token绑定

![image-20231204152641867](./image-20231204152641867.png)





