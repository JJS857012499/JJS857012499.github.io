---
title: Node.js 第一个应用
date: 2017-03-13 00:00:21
categories: Node.js
tags: [Node.js]
---
1、示例代码
<!-- more -->
``` shell
//使用 require 指令来载入 http 模块，并将实例化的 HTTP 赋值给变量 http
var http = require('http');
// http.createServer() 方法创建服务器，并使用 listen 方法绑定 8888 端口。 函数通过 request, response 参数来接收和响应数据。
http.createServer(function(request,response){
	response.writeHead(200,{'Content-Type':'text/plain'});
	response.write('<body>呵呵</body>');
	response.end('hello world\n');
}).listen(8888);
console.log('Server running...')
```
2、打开浏览器访问 http://127.0.0.1:8888/，大功告成
