# vue-configed
my vue template configed


vue 2.0 踩坑
  1.vue 2.0 的 webpackconfig.js 重命名了vue.js的导入名称导致的问题
    解决方法：https://segmentfault.com/a/1190000006435886
  2.webpack + vuejs 基本配置
    借鉴用例：http://www.cnblogs.com/lhweb15/p/5660609.html
    github地址：https://github.com/laneyhui/webpack-vuejs
    解决版本冲突：https://github.com/yaoyao1987/vue-cli-multipage
  3.配置
    http://www.cnblogs.com/brandonhulala/p/6057378.html
  4.Vue.js结合vue-router和webpack编写单页路由项目 
    http://blog.csdn.net/sinat_17775997/article/details/52548973
  5.知乎
    https://www.zhihu.com/question/39025104
  6.github 路由全介绍
    https://github.com/shinygang/Vue-cnodejs/blob/master/src/routers.js
  7.vue 2.0 填坑
    http://blog.csdn.net/sinat_17775997/article/details/52788908
  8.myself config
    package.json:
    "dependencies": {
        "babel-runtime": "^6.0.0",
        "jquery": "^2.1.1",
        "scriptjs": "^2.5.8",
        "vue": "^1.0.21",
        "vue-router": "^0.7.10",
        "vuex": "^0.8.2"
    },
    main.js:
    import Vue from 'vue';//加载vue.js
    import VuexStore from './vuex/store' ;//vuex状态管理，组件之间的通信
    import VueRouter from 'vue-router' ;//加载vue-router.js路由
    import RouterConfig from './router' ;//加载vue-router的定义文件
    // import AppConfig from './App';

    Vue.use(VueRouter);//使用路由插件
    //路由器需要创建一个根实例
    const App = Vue.extend({
        data:function(){
            return {
                siteTitle:'',
                apiUrl:''
            }
        },
        store: VuexStore
    });
    //创建一个路由器实例
    const router = new VueRouter({
        hashbang:false,//去除#！开头
        transitionOnload:false,//初次加载的时候是否对<router-view>处理场景切换效果
        history:true,//是否开启html5模式，去掉锚点
        saveScrollPosition:false //记住页面的滚动位置 html5模式适用
        // ,root:'/dist'//路由生效的基础路径
    });

    //定义路由规则
    router.map(RouterConfig);
    //路由重定向
    router.redirect({
      '*': '/error'//404页面
    });

    //路由会创建一个实例，挂载到相应的选择符匹配元素上
    router.start(App,'html');
