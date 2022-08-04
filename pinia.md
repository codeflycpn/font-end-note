# pinia

优秀的vue3状态管理



- 第一节讲了Pinia 的优势和环境的安装
- 第二节使用Pinia的方式创建一个Store
- 第三节用简单方法改变状态数据和你需要注意的事情
- 第四节Pinia改变状态数据的四种方式包括Actions的使用
- 第五节讲一下Pinia种Getters的使用
- 第六节讲Store的互相调用方法
- 第七节讲Pinia在vue-devtools中的调试方法





## 一、环境搭建和安装pinia

1. 使用vite搭建一个vue3+ts的项目

   1. 新建一个文件夹pinia

   2. vscode进入该文件夹，进入终端，初始化npm环境

      ```shell
      npm install vite@latest 
      ```

      要走一套流程，最后留下项目文件夹，运行项目

      ```
      ~pinia cd pinia-dome
      npm install 安装依赖
      npm run dev
      ```

      

2. 在这项目中安装pinia

   进入文件夹中安装pinia

   建议参看文档，`npm install pinia`、`yarn add pinia`

## 二、使用pinia仓库映射到组件上

1. 在main中引入pinia到全局

2. 新建store文件夹，也就是状态仓库，新建`index.ts`文件作为状态管理的入口

3. 导出index.ts中的状态，在组件中去引入它，并且调用状态值

   main.ts

   ```ts
   // -----main.ts
   
   import { createApp } from 'vue'
   import App from './App.vue'
   import {createPinia} from 'pinia'
   
   // 创建pinia实例
   const pinia = createPinia()
   const app = createApp(App)
   // 挂载插件
   app.use(pinia)
   // 挂载dom
   app.mount('#app')
   ```

   store/index.ts

   ```ts
   // -----store/index.ts
   
   // 状态管理的总局
   
   // 这里会有多个容器需要我们管理
   
   import { defineStore} from 'pinia'
   
   
   // 以下的写法都是固定的，可以配置代码段
   export const mainStore = defineStore('main',{
   
   
       // 这里全程就像是vue的小组件
       // state类型data中的数据
       state:()=>{
           return {
               msg:'hello pinia'
           }
       },
       getters:{},
       actions:{},
   
   })
   
   /**
    * defindeStore函数
    * main：就是一个名字，一块内存地址，唯一不可重复
    * 第二个参数：就是配置选项
    * 
    * state：这里就是全局状态了
    * getter：监视，状态值得变化，类似计算属性，具有缓存功能
    * actions：就是methons，里头都是函数
    * 
    * 其实以上东西的备注都没啥意义，在实践中才能深刻的体会其中的作用
    */
   ```

   组件

   ```vue
   <template>
     <h2 class="">{{ store.msg}}</h2>
   </template>
   
   <script lang="ts" setup>
   
   import { mainStore } from "../store/index"
   
   const store = mainStore();
   
   
   </script>
   
   <style lang="scss" scoped></style>
   ```

4. 坑爹的地方：
   * 引入组件会报错，需要在vscode中的vuter-script√去掉
   * 注意所有文件都是ts

## 三、修改状态数据

1.在首页添加一个组件，这个组件唯一作用就是修改store的数据，点击按钮触发，修改状态的方法

```vue
<template>
  <div>
      <button @click="handleClick">点击增加</button>
  </div>
</template>

<script lang="ts" setup>
import { mainStore } from "../store/index";
const store = mainStore();

// 点击增加count的方法

const handleClick = () => {
    // 修改单个数据
    store.count++
}
</script>

<style lang="scss" scoped></style>
```

2. 取出多个状态的值的方式

   ```js
   
   import { storeToRefs } from "pinia"; // 需要解构就用这个
   
   const store = mainStore();
   
   // 错误做法：const { msg,count } = store  通过这种方式取出来的数据不会更新
   
   // 正确做法  const { msg,count } = storeToRefs(store) 其实就是vue3的代理ref什么的
   
   ```

3. 总结

## 四、修改状态的四种方式

```vue
<template>
  <div>
      <button @click="handleClick">点击增加</button>
  </div>
</template>

<script lang="ts" setup>
import { mainStore } from "../store/index";
const store = mainStore();

// 点击增加count的方法

const handleClick = () => {
    // -- 第一种
    // 修改单个数据
    // store.count++
    // 修改多个数据，它这里有个底层优化，上面是该一下就更新视图，下面是全部改完在更新视图


    // 第二种
    // store.$patch({
    //     count:store.count + 2,
    //     msg:'我的发'
    // })

    // 第三种，适合比较复杂的操作，比如数组操作
    // store.$patch(state => {

    //     // state 就是状态数据源
    //     state.count++
    //     state.msg = '麻辣隔壁'

    // })

    // 第四种，直接状态里预制的函数，适合复杂情况

       store.changeState()
}

</script>

<style lang="scss" scoped></style>
```

第四种需要在状态中心的actions中预制一下

```ts
// 状态管理的总局

// 这里会有多个容器需要我们管理

import { defineStore} from 'pinia'


// 以下的写法都是固定的，可以配置代码段
export const mainStore = defineStore('main',{


    // 这里全程就像是vue的小组件
    // state类型data中的数据
    state:()=>{
        return {
            msg:'hello pinia',
            count:11,
        }
    },
    getters:{},
    actions:{

        // 这里有多个复杂操作数据的方法集成在这里，用来被前台调的
        changeState(){
            this.count ++
            this.msg = '我回来了'
        }

    },

})

```

## 五、getter的使用

就是计算属性，具有缓存性，只会计算一次，当状态值发生变化才会再次计算

有两种写法

```ts
// 状态管理的总局

// 这里会有多个容器需要我们管理

import { defineStore} from 'pinia'


// 以下的写法都是固定的，可以配置代码段
export const mainStore = defineStore('main',{


    // 这里全程就像是vue的小组件
    // state类型data中的数据
    state:()=>{
        return {
            msg:'hello pinia',
            count:11,
            phone:13599533943, // 手机号码
        }
    },
    // 就是计算属性：具有缓存性，不会重复执行计算，除非状态值发生了变化
    + getters:{
        // 第一种写法
            // phoneHidden(state){
            // console.log('PhoneHidden被调用了');
            // return state.phone.toString().replace(/^(\d{3})\d{4}(\d{4})$/, '$1****$2')
            // }
        // 第二种写法:如果不传state，ts无法推到要返回的类型，所以要补一个返回类型
            phoneHidden():string{
                console.log('PhoneHidden被调用了');
                return this.phone.toString().replace(/^(\d{3})\d{4}(\d{4})$/, '$1****$2')
            }
        
      },
    actions:{

        // 这里有多个复杂操作数据的方法集成在这里，用来被前台调的
        changeState(){
            this.count ++
            this.msg = '我回来了'
        }

    },

})

 */
```



## 六、store之间的相互调用

1. 新建一个状态屋，/store/lqs.ts

2. 获取状态状态屋的状态：先到入状态库再使用函数

   ```ts
   // 这里会有多个容器需要我们管理
   
   import { defineStore} from 'pinia'
   
   // 获取隔壁的状态屋+++
   import { lqs } from './lsq';
   
   
   // 以下的写法都是固定的，可以配置代码段
   export const mainStore = defineStore('main',{
   
   
       // 这里全程就像是vue的小组件
       // state类型data中的数据
       state:()=>{
           return {
               msg:'hello pinia',
               count:11,
               phone:13599533943, // 手机号码
           }
       },
       // 就是计算属性：具有缓存性，不会重复执行计算，除非状态值发生了变化
       getters:{}
           
         },
       actions:{
           // 获取其他屋的状态 +++ 语法，这个语法还是需要理解下
           getList(){
               console.log(lqs().list)
           }
   
       },
   
   })
   ```

   

## 七、pinia在vue-devtools可视化面板的调试

