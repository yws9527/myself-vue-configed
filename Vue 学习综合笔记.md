1.vue-resource学习
  
	  1）. Vue.http.get(uri,{[options]})
    
        请求需要发送的参数和请求头必须要放在后边的对象里边，然后具体的属性是以键值对的方式设置的，
        
        s特别注意：：：属性都有各自固定的键值名字，不可更改，有别于jquery的ajax请求参数，具体详情，建议参考vue-resource官方文档
        
	  2). 多种请求方式：
    
        局部请求：
      
          this.http.get(uri,{[options]})  ——— GET 方式 
         
          this.http.post(uri,{[options]}) ——— POST 方式 
         
          this.http({
	             // ..set requert content here
	             method : "GET/POST" —— GET / POST请求	
          })
         
        全局请求：
        
          Vue.http.get(uri,{[options]})  ——— GET 方式 
          
          Vue.http.post(uri,{[options]}) ——— POST 方式
         
          Vue.http({
	            // ..set requert content here
	            method : "GET/POST" —— GET / POST请求	
          })
         
        ss特别注意：：：
      
           GET  请求方式的参数放在 parmas 对象里
          
           POST 请求方式的参数放在 body 对象里
2.//获取客户端操作系统类型 

          function getectOSInfo () { 
              var sUserAgent = navigator.userAgent; 
              var isWin = (navigator.platform == "Win32") || (navigator.platform == "Windows"); 
              var isMac = (navigator.platform == "Mac68K") || (navigator.platform == "MacPPC") || (navigator.platform == "Macintosh") || (navigator.platform == "MacIntel"); 
              if (isMac) return "Mac"; 
              var isUnix = (navigator.platform == "X11") && !isWin && !isMac; 
              if (isUnix) return "Unix"; 
              var isLinux = (String(navigator.platform).indexOf("Linux") > -1); 
              if (isLinux) return "Linux"; 
              if (isWin) { 
                var isWin2K = sUserAgent.indexOf("Windows NT 5.0") > -1 || sUserAgent.indexOf("Windows 2000") > -1; 
                if (isWin2K) return "Win2000"; 
                var isWinXP = sUserAgent.indexOf("Windows NT 5.1") > -1 || sUserAgent.indexOf("Windows XP") > -1; 
                if (isWinXP) return "WinXP"; 
                var isWin2003 = sUserAgent.indexOf("Windows NT 5.2") > -1 || sUserAgent.indexOf("Windows 2003") > -1; 
                if (isWin2003) return "Win2003"; 
                var isWin2003 = sUserAgent.indexOf("Windows NT 6.0") > -1 || sUserAgent.indexOf("Windows Vista") > -1; 
                if (isWin2003) return "WinVista"; 
                var isWin2003 = sUserAgent.indexOf("Windows NT 6.1") > -1 || sUserAgent.indexOf("Windows 7") > -1; 
                if (isWin2003) return "Win7"; 
              } ;
              return "None"; 
            };
    
	    
3.//获取浏览器名字+版本字符串

            function getBrowserInfo (){
                var agent = navigator.userAgent.toLowerCase() ;
                var regStr_ie = /msie [\d.]+;/gi ;
                var regStr_ff = /firefox\/[\d.]+/gi
                var regStr_chrome = /chrome\/[\d.]+/gi ;
                var regStr_saf = /safari\/[\d.]+/gi ;
                //IE
                if(agent.indexOf("msie") > 0){
                    return agent.match(regStr_ie) ;
                };
                //firefox
                if(agent.indexOf("firefox") > 0){
                    return agent.match(regStr_ff) ;
                };
                //Chrome
                if(agent.indexOf("chrome") > 0){
                    return agent.match(regStr_chrome) ;
                };
                //Safari
                if(agent.indexOf("safari") > 0 && agent.indexOf("chrome") < 0){
                    return agent.match(regStr_saf) ;
                }
            };
            /*function locationHref(e){
                 switch(e){
                     case 'Mac':
                         window.location.href='http://www.baidu.com';
                         break;
                     case 'Unix':
                         window.location.href='http://tieba.baidu.com';
                         break;
                     case 'Linux':
                         window.location.href='http://map.baidu.com';
                         break;
                     case 'Win2000':
                         window.location.href='http://mp3.baidu.com';
                         break;
                     case 'WinXP':
                         window.location.href='http://baike.baidu.com';
                         break;
                     case 'Win2003':
                         window.location.href='http://zhidao.baidu.com';
                         break;
                     case 'WinVista':
                         window.location.href='http://hi.baidu.com';
                         break;
                     case 'Win7':
                         window.location.href='http://video.baidu.com';
                         break;
                     default :
                         window.location.href='http://www.hao123.com/';
                         break;
                 }
            };*/
