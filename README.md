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
    <title>videoBars</title>
    <script src="js/jquery-1.12.4.js"></script>
    <script src="js/vue-1.0.28.js"></script>
    <style>
        body {
            background: #ccc;
        }

        .inner {
            width: 800px;
            height: 450px;
            margin: 0 auto;
            position: relative;
            background: #666;
            border-bottom: 2px solid #fff;
        }

        .defaultBars {
            min-width: 800px;
            height: 32px;
            list-style-type: none;
            background-color: rgba(255, 140, 0, 0.5);
            color: #ddd;
            font-size: 10px;
            padding: 0;
            margin: 0;
            position: absolute;
            bottom: 0px;
        }

        .defaultBars li {
            float: left;
            display: inline-block;
            height: 32px;
            line-height: 22px;
        }

        .w-3 {
            width: 3%;
        }

        .w-5 {
            width: 5%;
        }

        .w-9 {
            width: 9%;
        }
        .w-11 {
            width: 11%;
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
        .noVolume,
        .volumeBar{
            padding: 5px;
        }

        .speedBtn {
            width: 32px;
            height: 18px;
            cursor: pointer;
            padding: 0;
        }
        .spd{
            line-height: 32px !important;
        }
        .timeS {
            height: 32px;
            line-height: 32px;
        }

        .timeXayi,.volumeBar {
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

        input[type="range"] {
            width: 100%;
            height: 1px;
            background: #beeabc;
            border-radius: 5px;
            -webkit-appearance: none !important;
            -moz-appearance: none !important;
            -ms-appearance: none !important;
            -o-appearance: none !important;
            outline: none;
        }

        input[type="range"]::-webkit-slider-thumb {
            width: 9px;
            height: 9px;
            background: #fff;
            cursor: pointer;
            border-radius: 50%;
            -webkit-box-shadow: 0 0 2px #eee;
            -webkit-appearance: none !important;
        }
        input[type="range"]::-moz-slider-thumb {
            width: 9px;
            height: 9px;
            background: #fff;
            cursor: pointer;
            border-radius: 50%;
            -moz-box-shadow: 0 0 2px #eee;
            -moz-appearance: none !important;
        }
        input[type="range"]::-ms-slider-thumb {
            width: 9px;
            height: 9px;
            background: #fff;
            cursor: pointer;
            border-radius: 50%;
            -ms-box-shadow: 0 0 2px #eee;
            -ms-appearance: none !important;
        }
        input[type="range"]::-o-slider-thumb {
            width: 9px;
            height: 9px;
            background: #fff;
            cursor: pointer;
            border-radius: 50%;
            -o-box-shadow: 0 0 2px #eee;
            -o-appearance: none !important;
        }
    </style>
</head>

<body>
    <div class="inner">
        <video id="oVideo" :width="videoWidth" :height="videoHeight" poster="images/logo.png"  @timeupdate="timeChange">
            <source src="videos/The Blue Birds.mp4" type="video/mp4">
            <source src="videos/The Blue Birds.ogg" type="video/ogg">
            <source src="videos/The Blue Birds.webm" type="video/webm">
            <object data="videos/The Blue Birds.mp4" :width="videoWidth" :height="videoHeight">
                <embed src="videos/The Blue Birds.swf" :width="videoWidth" :height="videoHeight">
            </object>
            Your browser does not support HTML5 video.
        </video>
        <ul class="defaultBars">
            <li class="w-3" title="播放/暂停">
                <button @click="actPlay" class="playButton"></button>
            </li>
            <li class="w-11" title="时间显示">
                <div class="timeS">
                    <span class="currentTime">{{cTime |tFormate}}</span>/<span class="totalTime">{{tTime |tFormate}}</span>
                </div>
            </li>
            <li class="w-5 spd" title="播放速度">
                <button @click="oSpeedChange" class="speedBtn">{{speed | spdForamate}}X</button>
            </li>
            <li class="w-60" title="时间轴">
                <input type="range" v-model="xay" @click="ojdChange" step="1" min="0" :max="totalTime" class="timeXayi" />
            </li>
            <li class="w-3" title="静音控制" @click="actIsVol($event)">
                <button class="noVolume"></button>
            </li>
            <li class="w-9" title="音量调节" @click="actIsVol($event)">
                <input type="range" v-model="oVol" step="0.1" min="0.0" max="1.0" class="volumeBar" />
            </li>
            <li class="w-3" title="全屏">
                <button @click="actFscreen" class="fullScreen"></button>
            </li>
        </ul>
    </div>
    <script>
    var myVideo = document.getElementById("oVideo");
    var videobox = document.querySelector(".inner");
    var defaultBars = document.querySelector(".defaultBars");

    new Vue({
        el: '.inner',
        data: {
            isPlay: false,
            videoWidth: 800,
            videoHeight: 450,
            currentTime: "00:00",
            totalTime: "",
            xay: 0,
            isFscreen: false,
            isNoVol:false,
            oVol:0.2,
            speed:1.0,
        },
        ready: function() {
            var vm = this;
            $(myVideo).on("loadedmetadata play pause", function(event) {
                if(event.type=="loadedmetadata"){
                    myVideo.defaultMuted=true;
                    myVideo.preload="metadata";
                    myVideo.autoplay=false;
                    vm.totalTime = this.duration;
                    vm.currentTime = this.currentTime;
                };
                if (event.type=="play") {
                    $(videobox).on("mouseenter mouseleave",function(event){
                        if (event.type=="mouseleave") {
                            $(defaultBars).slideUp();
                        }else{
                            $(defaultBars).slideDown();
                        }
                    });
                };
                if (event.type=="pause") {
                    $(defaultBars).slideDown();
                    event.stopPropagation();
                    $(videobox).unbind("mouseleave");
                };
            });
        },
        methods: {
            actPlay: function() {
                !this.isPlay ? myVideo.play() : myVideo.pause();
                this.isPlay = !this.isPlay;
                myVideo.volume=this.oVol;
            },
            timeChange: function(){
                this.currentTime = myVideo.currentTime;
                this.xay=myVideo.currentTime;
            },
            actFscreen: function() {
                this.checkState();
                if (!this.isFscreen) {
                    this.launchFullscreen(myVideo);
                } else {
                    this.exitFullscreen();
                }
            },
            //为全屏状态属性赋值
            checkState: function() {
                this.invokeFieldOrMethod(document, 'FullScreen');
                this.invokeFieldOrMethod(document, 'IsFullScreen');
                document.IsFullScreen;
            },
            //检查各浏览器全屏状态
            invokeFieldOrMethod: function(element, method) {
                ["webkit", "moz", "ms", "o", ""].forEach(function(prefix) {
                    if (this.isFscreen) return;
                    if (prefix === "") {
                        // 无前缀，方法首字母小写
                        method = method.slice(0, 1).toLowerCase() + method.slice(1);
                    };
                    var typePrefixMethod = typeof element[prefix + method];
                    if (typePrefixMethod + "" !== "undefined") {
                        if (typePrefixMethod === "function") {
                            this.isFscreen = element[prefix + method]();
                        } else {
                            this.isFscreen = element[prefix + method];
                        }
                    }
                });
            },
            //进入全屏
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
            actIsVol:function(event){
                if(event.target.nodeName=="BUTTON"){
                    this.isNoVol=!this.isNoVol;
                    myVideo.muted=this.isNoVol;
                }else{
                    myVideo.volume=this.oVol;
                }
            },
            ojdChange: function(){
                if (!myVideo.seeking) {
                    myVideo.currentTime=this.xay;
                    this.currentTime = myVideo.currentTime;
                };
                myVideo.play();
            },
            oSpeedChange: function(){
                this.speed=this.speed>=2?0.5:(this.speed+0.5);
                myVideo.playbackRate=this.speed;
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
                var mins = parseInt(val / 60) < 10 ? ("0" + parseInt(val / 60)) : (parseInt(val / 60));
                var seconds = parseInt(val % 60) < 10 ? ("0" + parseInt(val % 60)) : (parseInt(val % 60));
                return mins + ":" + seconds;
            },
            spdForamate:function(val){
                return val.toFixed(1);
            }
        }
    });
    </script>
</body>

</html>

  
  
  12.webpack.base.config 配置<br>
  <br>
    var path = require('path')<br>
    var webpack = require('webpack')<br>
    var config = require('../config')<br>
    var utils = require('./utils')<br>
    var projectRoot = path.resolve(__dirname, '../')<br>
    var autoprefixer = require('autoprefixer')<br>
    
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
    
    
 13.jsonp跨域的两种实现<br>
    http://m.toutiao.com/i6370891501244776961/?tt_from=weixin_moments&utm_campaign=client_share&from=timeline&app=news_article&utm_source=weixin_moments&iid=7040133467&utm_medium=toutiao_android&wxshare_count=1 <br>
 14.各个浏览器视频全屏的兼容写法 <br>
    https://my.oschina.net/ososchina/blog/349660 <br>
