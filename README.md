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
    };
    
  <br>
  10.webpack构建vue项目（配置篇）<br>
     http://www.cnblogs.com/brandonhulala/p/6057378.html<br>
  11.test-video<br>
    
      <!DOCTYPE html>
      <html>
      <head>
          <meta charset="utf-8">
          <meta http-equiv="X-UA-Compatible" content="IE=edge">
          <title>testVideo</title>
          <script src="js/jquery-1.12.3.js"></script>
          <script src="js/vue.js"></script>
          <style>
              body {
                  background: #ccc;
              }

              .inner {
                  display: inline-block;
                  position: absolute;
                  top: 0;
                  left: 0;
              }

              .defaultBars {
                  min-width: 800px;
                  height: 32px;
                  list-style-type: none;
                  background-color: rgba(255, 255, 255, 0.4);
                  color: #ddd;
                  font-size: 10px;
                  padding: 0;
                  margin: 0;
                  position: absolute;
                  bottom: 4px;
              }

              .defaultBars li {
                  float: left;
                  display: inline-block;
                  height: 32px;
                  line-height: 22px;
              }

              .w-5 {
                  width: 5%;
              }

              .w-10 {
                  width: 10%;
              }

              .w-60 {
                  width: 60%;
              }

              li {
                  margin-left: 5px;
                  text-align: center;
              }

              li:first-child {
                  margin-left: 8px;
              }

              .playButton,
              .fullScreen,
              .volumeBtn,
              .volumeBar {
                  padding: 9px;
              }

              .timeS {
                  height: 32px;
                  line-height: 32px;
              }

              .timeXayi {
                  width: 100%;
                  line-height: 32px;
                  height: 32px;
                  padding: 0;
                  margin: 0;
              }

              .currentTime {
                  margin-right: 2px;
              }

              .totalTime {
                  margin-left: 2px;
              }

              :-webkit-full-screen {}

              :-moz-full-screen {}

              :-ms-fullscreen {}

              :-o-fullscreen {}

              :full-screen {}

              :fullscreen {}

              :-webkit-full-screen video {
                  width: 100%;
                  height: 100%;
              }

              :-moz-full-screen video {
                  width: 100%;
                  height: 100%;
              }
          </style>
      </head>
      <body>
          <div class="inner">
              <video id="oVideo" :width="videoWidth" :height="videoHeight" poster="images/logo.png">
                  <source src="videos/movie0.mp4" type="video/mp4">
                  <source src="videos/movie0.ogg" type="video/ogg">
                  <source src="videos/movie0.webm" type="video/webm">
                  <object data="videos/movie0.mp4" :width="videoWidth" :height="videoHeight">
                      <embed src="videos/movie0.swf" :width="videoWidth" :height="videoHeight">
                  </object>
                  Your browser does not support HTML5 video.
              </video>
              <ul class="defaultBars">
                  <li class="w-5" title="播放">
                      <button @click="actPlay" class="playButton"></button>
                  </li>
                  <li class="w-10" title="时间显示">
                      <div class="timeS">
                          <span class="currentTime">{{cTime |tFormate}}</span>/<span class="totalTime">{{tTime |tFormate}}</span>
                      </div>
                  </li>
                  <li class="w-60" title="时间轴">
                      <input type="range" :value="xay" step="1" min="0" max="1000" class="timeXayi" />
                  </li>
                  <li class="w-5" title="声音控制">
                      <button class="volumeBtn"></button>
                  </li>
                  <li class="w-10" title="音量调节">
                      <button class="volumeBar"></button>
                  </li>
                  <li class="w-5" title="全屏">
                      <button @click="actFscreen" class="fullScreen"></button>
                  </li>
              </ul>
          </div>
          <script>
          var myVideo = document.getElementById("oVideo");
          var defaultBars = document.querySelector(".defaultBars");
          var startBtn = document.querySelector(".playButton");
          //var currentTime = document.querySelector(".currentTime");
          //var totalTime = document.querySelector(".totalTime");
          var timeXayi = document.querySelector(".timeXayi");
          var volumeBtn = document.querySelector(".volumeBtn");
          var volumeBar = document.querySelector(".volumeBar");
          var fullScreen = document.querySelector(".fullScreen");
          var videobox = document.querySelector(".inner");


          new Vue({
              el: '.inner',
              data: {
                  isPlay: false,
                  isFscreen: false,
                  videoWidth: 800,
                  videoHeight: 450,
                  currentTime: "0:00",
                  totalTime: "",
                  xay: 0,

              },
              ready: function() {
                  var vm = this;
                  $(myVideo).on("loadedmetadata timeupdate", function(event) {
                      if (event.type = "loadedmetadata") {
                          vm.totalTime = myVideo.duration;
                          vm.currentTime = this.currentTime;
                      };
                      if (event.type = "timeupdate") {
                          vm.currentTime = myVideo.currentTime;
                          vm.xay = (myVideo.currentTime / myVideo.duration) * 1000;
                      };
                  });
              },
              methods: {
                  actPlay: function() {
                      !this.isPlay ? myVideo.play() : myVideo.pause();
                      this.isPlay = !this.isPlay;
                  },
                  actFscreen: function() {

                      this.launchFullscreen(myVideo);
                      var vm=this;
                      function exit() {//检查浏览器是否处于全屏
                          if (vm.invokeFieldOrMethod(document, 'FullScreen') || vm.invokeFieldOrMethod(document, 'IsFullScreen') || document.IsFullScreen) {
                              vm.exitFullscreen();
                          }
                      };
                      window.setTimeout(exit, 5 * 1000);

                      /*if(!this.isFscreen){
                          this.videoWidth = window.screen.width;
                          this.videoHeight = window.screen.height;
                      }else{
                          this.videoWidth = 800;
                          this.videoHeight = 450;
                      };
                      $(defaultBars).width(this.videoWidth);
                      $(defaultBars).width(this.videoHidth);
                      this.isFscreen=!this.isFscreen;*/
                  },
                  invokeFieldOrMethod: function(element, method) {
                      var usablePrefixMethod;
                      ["webkit", "moz", "ms", "o", ""].forEach(function(prefix) {
                          if (usablePrefixMethod) return;
                          if (prefix === "") {
                              // 无前缀，方法首字母小写
                              method = method.slice(0, 1).toLowerCase() + method.slice(1);
                          };
                          var typePrefixMethod = typeof element[prefix + method];
                          if (typePrefixMethod + "" !== "undefined") {
                              if (typePrefixMethod === "function") {
                                  usablePrefixMethod = element[prefix + method]();
                              } else {
                                  usablePrefixMethod = element[prefix + method];
                              }
                          }
                      });
                      return usablePrefixMethod;
                  },
                  //進入全屏
                  launchFullscreen: function(element) {
                      //此方法不可以在異步任務中執行，否則火狐無法全屏
                      if (element.requestFullscreen) {
                          element.requestFullscreen();
                      } else if (element.mozRequestFullScreen) {
                          element.mozRequestFullScreen();
                      } else if (element.msRequestFullscreen) {
                          element.msRequestFullscreen();
                      } else if (element.oRequestFullscreen) {
                          element.oRequestFullscreen();
                      } else if (element.webkitRequestFullscreen) {
                          element.webkitRequestFullScreen();
                      } else {
                          var docHtml = document.documentElement;
                          var docBody = document.body;
                          var cssText = 'width:100%;height:100%;overflow:hidden;';
                          docHtml.style.cssText = cssText;
                          docBody.style.cssText = cssText;
                          videobox.style.cssText = cssText + ';' + 'margin:0px;padding:0px;';
                          document.IsFullScreen = true;
                      }
                  },
                  //退出全屏
                  exitFullscreen: function() {
                      if (document.exitFullscreen) {
                          document.exitFullscreen();
                      } else if (document.msExitFullscreen) {
                          document.msExitFullscreen();
                      } else if (document.mozCancelFullScreen) {
                          document.mozCancelFullScreen();
                      } else if (document.oRequestFullscreen) {
                          document.oCancelFullScreen();
                      } else if (document.webkitExitFullscreen) {
                          document.webkitExitFullscreen();
                      } else {
                          var docHtml = document.documentElement;
                          var docBody = document.body;
                          docHtml.style.cssText = "";
                          docBody.style.cssText = "";
                          videobox.style.cssText = "";
                          document.IsFullScreen = false;
                      }
                  },
              },
              computed: {
                  cTime: function() {
                      this.currentTime = myVideo.currentTime;
                      return this.currentTime;
                  },
                  tTime: function() {
                      this.totalTime = myVideo.duration;
                      return this.totalTime;
                  },
              },
              filters: {
                  tFormate: function(val) {
                      var mins = parseInt(val / 60);
                      var seconds = parseInt(val % 60);
                      return mins + ":" + (seconds < 10 ? "0" + seconds : seconds);
                  }
              }
          });
          </script>
      </body>
      </html>
  12.webpack.base.config 配置<br>
  
    var path = require('path')
    var webpack = require('webpack')
    var config = require('../config')
    var utils = require('./utils')
    var projectRoot = path.resolve(__dirname, '../')
    var autoprefixer = require('autoprefixer')
    module.exports = {
      entry: {
        app: './src/main.js'
      },
      output: {
        path: config.build.assetsRoot,
        //  path:'../dist',

        //根目录开启
        publicPath: process.env.NODE_ENV === 'production' ? config.build.assetsPublicPath : config.dev.assetsPublicPath,
        // publicPath:'./',
        // publicPath:'/dist/',//如果不是根目录开启,写路径

        filename: '[name].js'
      },
      resolve: {
        extensions: ['', '.js', '.vue'],
        fallback: [path.join(__dirname, '../node_modules')],
        alias: {
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
            test: /\.html$/,
            loader: 'vue-html'
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
      plugins:[
        new webpack.ProvidePlugin({
            $:"jquery",
            jQuery:"jquery",
            jquery:"jquery"
        }),
        new webpack.optimize.CommonsChunkPlugin('common.js')
      ],
      vue: {
        loaders: utils.cssLoaders()
      }
      ,postcss:function(){
        return [require("autoprefixer")];
      }
    }

