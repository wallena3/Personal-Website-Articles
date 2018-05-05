登录有一个图，很直观，看下面。主要分三步，请求code，请求access_code,请求详细信息  
跨域请求纯前端最终的解决办法:使用一个公开的代理  
请求的发出：在header中发出请求登录的action，在container中带上当前页面的网址链接，达到当前页面不跳转的效果  
这个重定向的地址必须和github中callbackURL有关联，callbackURL应该为重定向地址的子串，或者相等  
使用localstorage存储登录信息，达到持久化效果。  
在最外层的App组件中，componentWillMount中加入判断登录，登录的后续逻辑，可以达到任意页面登录，执行后续登录步骤的