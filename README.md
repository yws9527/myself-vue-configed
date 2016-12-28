# vue-configed
my vue template configed


vue 2.0 踩坑<br>
  1.vue 2.0 的 webpackconfig.js 重命名了vue.js的导入名称导致的问题<br>
    解决方法：https://segmentfault.com/a/1190000006435886<br>
  2.webpack + vuejs 基本配置<br>
    借鉴用例：http://www.cnblogs.com/lhweb15/p/5660609.html<br>
    github地址：https://github.com/laneyhui/webpack-vuejs<br>
    解决版本冲突：https://github.com/yaoyao1987/vue-cli-multipage<br>
  3.配置<br>
    http://www.cnblogs.com/brandonhulala/p/6057378.html<br>
  4.Vue.js结合vue-router和webpack编写单页路由项目 <br>
    http://blog.csdn.net/sinat_17775997/article/details/52548973<br>
  5.知乎<br>
    https://www.zhihu.com/question/39025104<br>
  6.github 路由全介绍<br>
    https://github.com/shinygang/Vue-cnodejs/blob/master/src/routers.js<br>
  7.vue 2.0 填坑<br>
    http://blog.csdn.net/sinat_17775997/article/details/52788908<br>
  8.myself config<br>
  
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
    
  9.webpack 配置<br>
    http://www.cnblogs.com/bergus/p/4820435.html<br>
    http://yijiebuyi.com/blog/46fb97b11fb8f4055e0b04d1cecb1f69.html<br>
    我的配置：
    
    var path = require('path')
    var config = require('../config')
    var utils = require('./utils')
    var webpack = require('webpack')
    var projectRoot = path.resolve(__dirname, '../')
    var autoprefixer =require('autoprefixer')
    var env = process.env.NODE_ENV
    // check env & config/index.js to decide weither to enable CSS Sourcemaps for the
    // various preprocessor loaders added to vue-loader at the end of this file
    var cssSourceMapDev = (env === 'development' && config.dev.cssSourceMap)
    var cssSourceMapProd = (env === 'production' && config.build.productionSourceMap)
    var useCssSourceMap = cssSourceMapDev || cssSourceMapProd
    <br><br>
    module.exports = {
      entry: {
        app: './src/main.js'
      },
      output: {
        path: config.build.assetsRoot,
        publicPath: process.env.NODE_ENV === 'production' ? config.build.assetsPublicPath : config.dev.assetsPublicPath,
        filename: '[name].js'
      },
      resolve: {
        extensions: ['', '.js', '.vue'],
        fallback: [path.join(__dirname, '../node_modules')],
        alias: {
          'vue$': 'vue/dist/vue.js',
          'bootstrap-vue': '../node_modules/bootstrap-vue/build/bootstrap-vue.js',
          'src': path.resolve(__dirname, '../src'),
          'assets': path.resolve(__dirname, '../src/assets'),
          'components': path.resolve(__dirname, '../src/components')
        }
      },
      resolveLoader: {
        fallback: [path.join(__dirname, '../node_modules')]
      },
      module: {
        loaders: [
          {
            test: /\.vue$/,
            loader: 'vue'
          },
          {
            test: /\.js$/,
            loader: 'babel',
            include: projectRoot,
            exclude: /node_modules/
          },
          {
            test: /\.json$/,
            loader: 'json'
          },
          {
            test: /\.(png|jpe?g|gif|svg)(\?.*)?$/,
            loader: 'url',
            query: {
              limit: 10000,
              name: utils.assetsPath('img/[name].[hash:7].[ext]')
            }
          },
          {
            test: /\.(woff2?|eot|ttf|otf)(\?.*)?$/,
            loader: 'url',
            query: {
              limit: 10000,
              name: utils.assetsPath('fonts/[name].[hash:7].[ext]')
            }
          }
        ]
      },
      eslint: {
        formatter: require('eslint-friendly-formatter')
      },
      plugins: [
          new webpack.DefinePlugin({// definePlugin 接收字符串插入到代码当中, 所以你需要的话可以写上 JS 的字符串
              __DEV__: JSON.stringify(JSON.parse(process.env.BUILD_DEV || 'true')),
              __PRERELEASE__: JSON.stringify(JSON.parse(process.env.BUILD_PRERELEASE || 'false'))
          }),
          new webpack.optimize.UglifyJsPlugin(),
          new webpack.optimize.CommonsChunkPlugin('common.js'),
          new webpack.ProvidePlugin({
              $: "jquery",
              jQuery: "jquery",
              "window.jQuery": "jquery"
          })
      ],
      vue: {
        loaders: utils.cssLoaders({ sourceMap: useCssSourceMap }),
        postcss: [
          require('autoprefixer')({
            browsers: ['last 2 versions']
          })
        ]
      }
    }

    10.webpack构建vue项目（配置篇）
      http://www.cnblogs.com/brandonhulala/p/6057378.html
