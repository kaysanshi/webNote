## Vue和angular项目：

### Vue项目2.x创建

- 安装nodejs
- 使用cnpm :npm install -g cnpm --registry=https://registry.npm.taobao.org
- 搭建vue环境： npm install --global vue-cli  /   cnpm install --global vue-cli 
- 创建项目：vue init webpack 项目名称  ...一系列的操作
- 运行： npm run dev

### vue3.x创建

1. npm uninstall -g vue-cli
2. npm install -g @vue/cli
3. vue create 项目名称

https://blog.csdn.net/qq_43459332/article/details/102487268

#### Vue项目目录：

1. build 文件夹，用来存放项目构建脚本

2. config 中存放项目的一些基本配置信息，最常用的就是端口转发

3. node_modules 这个目录存放的是项目的所有依赖，即 npm install 命令下载下来的文件

4. src 这个目录下存放项目的源码，即开发者写的代码放在这里

5. static 用来存放静态资源

6. index.html 则是项目的首页，入口页，也是整个项目唯一的HTML页面

7. package.json 中定义了项目的所有依赖，包括开发时依赖和发布时依赖

   ##### src 目录：

   1. assets 目录用来存放资产文件

   2. components 目录用来存放组件（一些可复用，非独立的页面），当然开发者也可以在 components 中直接创建完整页面。

   3. 推荐在 components 中存放组件，另外单独新建一个 page 文件夹，专门用来放完整页面。

   4. router 目录中，存放了路由的js文件

   5. App.vue 是一个Vue组件，也是项目的第一个Vue组件

   6. main.js相当于Java中的main方法，是整个项目的入口js

      ###### # main.js

      1. 在main.js 中，首先导入 Vue 对象
      2. 导入 App.vue ，并且命名为 App
      3. 导入router，注意，由于router目录下路由默认文件名为 index.js ，因此可以省略
      4. 所有东西都导入成功后，创建一个Vue对象，设置要被Vue处理的节点是 '#app'，'#app' 指提前在index.html 文件中定义的一个div
      5. 将 router 设置到 vue 对象中，这里是一个简化的写法，完整的写法是 router:router，如果 key/value 一模一样，则可以简写。
      6. 声明一个组件 App，App 这个组件在一开始已经导入到项目中了，但是直接导入的组件无法直接使用，必须要声明。
      7. template 中定义了页面模板，即将 App 组件中的内容渲染到 '#app' 这个div 中。

#### 使用安装vue -router

   1.安装 vue-router ： npm install vue-router --save /cnpm install vue -router --save

2. main.js中引入  ： import VueRouter from 'vue-router'  Vue.use(VueRouter)

3. 配置路由		


   	   1、创建组件 引入组件
   	   2、定义路由  （建议复制s）
   			const routes = [
   			  { path: '/foo', component: Foo },
   			  { path: '/bar', component: Bar },
   			  { path: '*', redirect: '/home' }   /*默认跳转路由*/
   			]
   		3、实例化VueRouter
   			const router = new VueRouter({
   			  routes // （缩写）相当于 routes: routes
   			})
   	
   		4、挂载
   		new Vue({
   		  el: '#app',
   		  router，
   		  render: h => h(App)
   		})
   		5 、根组件的模板里面放上这句话   <router-view></router-view>    
   	    6、路由跳转
   		<router-link to="/foo">Go to Foo</router-link>
   		 <router-link to="/bar">Go to Bar</router-link>
   	     
   	 实例   
   	import Vue from 'vue'
   	import Router from 'vue-router'
   	import HelloWorld from '@/components/HelloWorld'
   	import BrotherQ from'@/components/brothe/BrotherQ'
   	import Content from '@/components/router-get/content'
   	import Content2 from '@/components/router-get/Content2'
   	
   	import List from '@/components/router-get/list'
   	import ResourceList from '@/components/vue-resources/list'
   	import detail from '@/components/vue-resources/detail'
   	
   	import pushList from '@/components/router-push/List'
   	import pushdetail from '@/components/router-push/Detail'
   	import User from '@/components/router-child/User'
   	import AddUser from '@/components/router-child/AddUser'
   	import ListUser from '@/components/router-child/UserList'
   	
   	import minitUI from '@/components/mintUI/index'
   	import ElementUIindex from '@/components/elementui/Elementuiindex'
   	
   	import ScollPage from '@/components/mintUI/ScollPage'
   	
   	import vuexindex from '@/components/vuex-vue/index'


   	
   	
   	Vue.use(Router)
   	// 先引入-->use()-->实例化new Router({}) 并且暴露出去
   	// 路由就是动态的将页面挂载到app.vue下 在main.js中引入并且挂载到vue中
   	
   	export default new Router({
   	  routes: [
   	    {
   	      path: '/',
   	      name: 'HelloWorld',
   	      component: HelloWorld
   	    },
   	    {
   	      path: '/brother',
   	      name: 'BrotherQ',
   	      component: BrotherQ
   	    },{
   	      path: '/list',
   	      name: 'List',
   	      component: List
   	    },{
   	      path:'/content/:aid', // 动态路由
   	      name:'content',
   	      component:Content
   	    },
   	    {
   	      path:'/content2', // 动态路由
   	      name:'content2',
   	      component:Content2
   	    },
   	    {
   	      path:'/resourelist',
   	      name:'ResourceList',
   	      component:ResourceList
   	    },{
   	      path:'/detail/:aid',
   	      name:'detail',
   	      component:detail
   	    },
   	    {
   	      path:'/pushList',
   	      name:'pushList',
   	      component:pushList
   	    },{
   	      path:'/pushdetail/:aid',
   	      name:'pushdetail',
   	      component:pushdetail
   	    },{
   	      path:'/user',
   	      name:'user',
   	      component:User,
   	      children:[{
   	        path:'adduser',component:AddUser
   	      },{
   	        path:'userlist',component:ListUser
   	      }
   	    ]
   	    },{
   	      path:'/index',
   	      name:'minitUI',
   	      component:minitUI
   	    },{
   	      path:"/ScollPage",
   	      name:'ScollPage',
   	      component:ScollPage
   	    },{
   	      path:'/eleindex',
   	      name:'ElementUIindex',
   	      component:ElementUIindex
   	    },{
   	      path:"/vuexindex",
   	      name:'vuexindex',
   	      component:vuexindex
   	    }
   	  ]
   	})



#### 使用elementUI和mintUI

		1.安装   npm install mint-ui -S         -S表示  --save
	
	    3.引入mint Ui的css 和 插件
	    
	    	import Mint from 'mint-ui';
	    
	    	Vue.use(Mint);
		   import 'mint-ui/lib/style.css'
		4.看文档直接使用。   
		
		// elementUI
		1.找官网  http://element.eleme.io/#/zh-CN/component/quickstart
	
		2.安装  cnpm i element-ui -S         -S表示  --save
	
		3.引入element UI的css 和 插件
	
			import ElementUI from 'element-ui';
			import 'element-ui/lib/theme-chalk/index.css';
			Vue.use(ElementUI);
		4、*webpack.config.js  配置file_loader      http://element.eleme.io/1.4/#/zh-CN/component/quickstart
				  {
					test: /\.(eot|svg|ttf|woff|woff2)(\?\S*)?$/,
					loader: 'file-loader'
			          }
			5.看文档直接使用。	

##  elementUI单独使用：

​	

			  ​	import { Button, Select } from 'element-ui';
	​	
	​		Vue.use(Button)
	​		Vue.use(Select)


​	
		
	​		引入对应的css
	​	
	​			import 'element-ui/lib/theme-chalk/index.css';
	​	
	​		如果报错： webpack.config.js  配置file_loader
	​	
			  {
				test: /\.(eot|svg|ttf|woff|woff2)(\?\S*)?$/,
				loader: 'file-loader'
		       }


#### 父子组件传值

父组件给子组件传值只需要在父组件中绑定一个值然后在子组件接收一些这个值 通过props:['title']

​    1.父组件传值给子组件，在父组件调用子组件时绑定动态属性

​    2.子组件通过props:['属性'] 这样就可以接收到了。

​    3.同时可以在子组件中定义要传入的值

​    4.可以直接传入父组件的方法，通过直接传入run方法

​    5.可以直接传入home组件的对象本身

 父组件主动获取子组件的数据和方法

​    1.调用子组件的时候定义一个ref

​    2.在父组件中通过以下放式直接调用

​      this.$refs.子组件名称.属性

​      this.$refs.子组件.方法

```
<template>
  <div class="hello">
    首页组件---我是父组件
    父组件给子组件传值只需要在父组件中绑定一个值然后在子组件接收一些这个值 通过props:['title']
    1.父组件传值给子组件，在父组件调用子组件时绑定动态属性
    2.子组件通过props:['属性'] 这样就可以接收到了。
    3.同时可以在子组件中定义要传入的值
    4.可以直接传入父组件的方法，通过直接传入run方法
    5.可以直接传入home组件的对象本身
    <v-header :title="title" :name="name" :run="run" :home="this" ref="header"></v-header>
    父组件主动获取子组件的数据和方法
    1.调用子组件的时候定义一个ref
    2.在父组件中通过以下放式直接调用
      this.$refs.子组件名称.属性
      this.$refs.子组件.方法
    <button @click="getChildData">获取子组件的数据和方法</button>
  </div>
</template>

<script>
import Header from './header.vue'
export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      title:'23333我是传入的值',
      name:'张飞 飞大灰机',
    }
  },
  components:{
    'v-header':Header
  },
  methods:{
     run(){
            alert('我是Home组件')
        },
     run_away(){
       alert('我是父组件中的定义的方法')
     },
     getChildData(){
       alert(this.$refs.header.msg)
       this.$refs.header.child() // 获取子组件的方法
     }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
```

子组件

```
<template>
    <div> 我是子组件 父组件传入的值为:::{{title}}
        <div>父组件绑定的值{{name}}</div>
        <!-- 父传子，子传孙，使用 v-bind="$attrs"-->
        <header-er :name="name" v-bind="$attrs"></header-er>
        <button @click="run('我是这个方法')">子组件调用父组件的方法</button>
        <button @click="getRun()">子组件调用父组件数据和方法</button>

    </div>
    
</template>
<script>
import header_er from './header_er.vue'
export default {
    props:['title','name','run','home'],
    data() {
        return {
            msg:'我是子组件data'
        }
    },
    methods:{
        getRun(){
            console.log(this.home)

            // 执行父组件的方法
            this.home.run()
            // 执行父组件的方法
            this.home.run_away()
            // 调用父组件的方法的形式
            this.$parent.run_away()
        },
        child(){
            alert('你廖勇了我的方法')
            console.log('我是开林俊杰')
        }
       
    },
    components:{
        'header-er':header_er
    }
}
</script>
<style scoped>

</style>

<!---孙子组件---->
<template>
    <div>
        <div>我是孙子组件 {{name}}</div>
    </div>

</template>
<script>
export default {
    name:'header-er',
    data(){
        return{

        }
    },
    props:{
       name :{
           type:String
       }
    }
}
</script>
<style scoped>

</style>
```

#### 兄弟组件传值

```
<template>
    <div>
        非父子组件传值：
        1.新建一个js文件，引入vue并实例化，暴露实例 export default Bus
        2.在要广播的地方引入刚才定义的实例
        3.通过 bus.$emit('name','data')
        4.在接受数据的地方通过$on进行监听并接受
            先引入bus.js
            bus.$on('name',function(){})
        <div>我是兄弟Q组件 </div>
        <button @click="sendMessage">给兄弟M发送消息</button>
    </div>

</template>
<script>
import bus from './Bus.js'
export default {
  
    data(){
        return{

        }
    },
    props:{
       
    },
    methods:{
        sendMessage(){
            bus.$emit('toBrothM','消息来一条')
        }
    }
}
</script>
<style scoped>

</style>
<!--- 中间的js暴露出实例Vue--->
import Vue from 'vue'

var bus=new Vue()

// 暴露出去这个vue
export default bus

<!----->
<template>
    <div>
        <div>我是兄弟M组件 </div>
    </div>

</template>
<script>
import bus from './Bus.js'

export default {
    data(){
        return{

        }
    },
    props:{
       
    },
    mounted(){
        bus.$on('toBrothM',function(data){
            console.log(data)
        })
    }
}
</script>
<style scoped>

</style>
```

#### 编程式导航

```
<template>
    <div>
        编程式导航
        使用 'router-link'创建的a标签来定义链接，我们还可以借助于router的实例方法
        注意：在 Vue 实例内部，你可以通过 $router 访问路由实例。因此你可以调用 this.$router.push。
        想要导航到不同的 URL，则使用 router.push 方法。这个方法会向 history 栈添加一个新的记录，所以，当用户点击浏览器后退按钮时，则回到之前的 URL。
        当你点击 router-link 时，这个方法会在内部调用，所以说，点击  router-link :to="..." 等同于调用 router.push(...)。
        可以使用以下方式：
            router.push('home')
            router.push({path:'home'})
            router.push({name:'user',params:{userId:'123'}}) 命名的路由
            router.push({path:'register',query:{plan:'private'}}) 带参数查询的
                const userId = '123'
                router.push({ name: 'user', params: { userId }}) // -> /user/123
                router.push({ path: `/user/${userId}` }) // -> /user/123
                // 这里的 params 不生效
                router.push({ path: '/user', params: { userId }}) // -> /user
        《<button @click="godetail()">通过js跳转到新闻页面</button>
        《<button @click="godetail2()">通过name定义js跳转到新闻页面2</button>


    </div>
</template>
<script>
export default {
    data() {
        return {
            
        }
    },
    methods: {
        // 两种跳转模式
        godetail(){
            this.$router.push({path:'pushdetail/44'}) 
        },
         godetail2(){
            const id='44'
            this.$router.push({name:'pushdetail',params:{aid:id}}) 
        }
    },
    mounted() {
        
    },
    components:{

    }
}
</script>
<style scoped>

</style>
```



### AngularJS项目创建

- 安装nodejs

- 使用cnpm :npm install -g cnpm --registry=https://registry.npm.taobao.org

- 搭建vue环境： npm install --global @angular/cli  /   cnpm install --global @angular/cli

- 创建项目：ng new projectName  可以跳过 依赖 ng new projectName --skip-install

- 当 文件都出现时 使用 cnpm install 更加快捷

- 运行： ng  serve --open

  /******

   \* angular6 使用socket.io 报错global is not defined 在 polyfills.ts中加入

   */

  (window as any).global = window;

  

#### angular 目录结构

e2e： 端到端测试

node_models 安装的第三方模块

src ：项目源码，开发所放置的代码

.angular-cli.json: Angular CLI的配置文件

package.json: npm配置的文件

browerslist: 

angular.json:

tslint.json：是tslint的配置文件，用来定义TypeScript代码质量检查的规则，不用管它

##### src目录

**app目录**             包含应用的组件和模块，我们要写的代码都在这个目录

 **assets目录**            资源目录，存储静态资源的  比如图片

 **environments目录**      环境配置。Angular是支持多环境开发的，我们可以在不同的环境下（开发环境，测试环境，生产环境）共用一套代码，主要用来配置环境的

 **index.html**          整个应用的根html，程序启动就是访问这个页面

 **main.ts**             整个项目的入口点，Angular通过这个文件来启动项目

 **polyfills.ts**        主要是用来导入一些必要库，为了让Angular能正常运行在老版本下

 **styles.css**          主要是放一些全局的样式

 **tsconfig.app.json**   TypeScript编译器的配置,添加第三方依赖的时候会修改这个文件

 **tsconfig.spec.json**  不用管

 **test.ts**             也是自动化测试用的

 **typings.d.ts**        不用管

##### app目录

app目录是我们要编写的代码目录。我们写的代码都是放在这个目录。
一个Angular程序至少需要一个模块和一个组件。在我们新建项目的时候命令行已经默认生成出来了。

**app.component.ts**：这个文件表示组件，组件是Angular应用的基本构建模块，可以理解为一段带有业务逻辑和数据的Html我们来看看app.component.ts中的代码，并解释下代码的意义**selecto**r就是css选择器，表示这个组件可以通过app-root的HTML页面标签来来调用，index.html中有个<app-root>Loading...</app-root>标签，这个标签用来展示该组件的内容

**app.module.ts** :这个文件是angular根模块，告诉angular如何组装应用，根模块不需导出任何东西，因为其他组件不需要导入 根模块

```
/**
 * 这个文件是angular根模块，告诉angular如何组装应用，
 * 根模块不需导出任何东西，因为其他组件不需要导入 根模块
 */
// BrowserModel 浏览器解析的模块
import { BrowserModule } from '@angular/platform-browser';
// Angular核心模块
import { NgModule } from '@angular/core';
// 路由
import { AppRoutingModule } from './app-routing.module';
// 根组件
import { AppComponent } from './app.component';

/**
 * NgModule装饰器，接受一个元数据对象，告诉Angular如何编译和启动应用
 */
@NgModule({
  // 配置当前运行的组件
  declarations: [
    AppComponent
  ],
  // imports配置当前模块运行依赖的其他模块
  imports: [
    BrowserModule,
    AppRoutingModule
  ],
  // 配置项目所需的服务
  providers: [],
  // 指定应用的主视图，通过引导根AppModule来启动应用这一般是写的是根组件
  bootstrap: [AppComponent]
})
export class AppModule { }

```

#### 创建组件并使用

 指定在哪个地方创建组件，同时指定创建的目录ng g component  components/home

使用命令创建组件会同时创建出四个文件：

- .html :书写 html代码文件

- .scss ： 书写css文件代码

- .spec.ts ：测试文件

- .ts ：相当于一个js文件，会自定义自身为一个组件，并且暴露出去，同时制定了组件的名称。和模板以及样式的路径。

  

创建组件的同时需要在app.module.ts进行挂载，挂载到根组件，

1.先是导入这个组件

```
// 引入了home组件
import { HomeComponent } from './components/home/home.component';
```

2.在装饰器中引入组件

```
/**
 * NgModule装饰器，接受一个元数据对象，告诉Angular如何编译和启动应用
 */
@NgModule({
  // 配置当前运行的组件
  // 引入组件
  declarations: [
    AppComponent,
    NewsComponent,
    HomeComponent
  ],
  // imports配置当前模块运行依赖的其他模块
  imports: [
    BrowserModule,
    AppRoutingModule
  ],
  // 配置项目所需的服务
  providers: [],
  // 指定应用的主视图，通过引导根AppModule来启动应用这一般是写的是根组件
  bootstrap: [AppComponent]
})
export class AppModule { }
```

