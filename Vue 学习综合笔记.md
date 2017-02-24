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
