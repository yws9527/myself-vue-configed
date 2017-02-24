1.vue-resource学习<br/><br/>
	  1）. Vue.http.get(uri,{[options]})  请求需要发送的参数和请求头必须要放在后边的对象里边，然后具体的属性是以键值对的方式设置的，<br/><br/>
				s特别注意：：：属性都有各自固定的键值名字，不可更改，有别于jquery的ajax请求参数，具体详情，建议参考vue-resource官方文档<br/><br/>
	  2). 多种请求方式：<br/><br/>
	  	局部请求：this.http.get(uri,{[options]})————get方式<br/><br/>
				 this.http.post(uri,{[options]})————post方式<br/><br/>
				 this.http({<br/>
				 	method:"GET/POST" —— GET/POST请求			
 				 })<br/><br/>		
		全局请求：Vue.http.get(uri,{[options]})————get方式<br/><br/>
				 Vue.http.post(uri,{[options]})————post方式<br/><br/>
				 Vue.http({<br/>
				 	method:"GET/POST" —— GET/POST请求			
 				 })<br/><br/>
			ss特别注意：：： GET  请求方式的参数放在 parmas 对象里<br/><br/>
						    POST 请求方式的参数放在 body 对象里<br/><br/>
