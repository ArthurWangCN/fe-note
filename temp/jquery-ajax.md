# Jquery Ajax 配置全局请求头 Header 信息及返回结果统一处理
前端发起 Ajax 请求，往往都有全局配置请求头 Header 信息的需求。比如请求发起时，希望针对所有的请求，放置用户 token 信息，其他数据等。
所有的 Ajax请求返回结果，当数据不符合预期时，统一拦截处理，更专注业务。
这样的需求很常见，不可能让前端开发人员对每一个请求都做配置，太累，也太 Low！

**解决方案：**
针对每一个页面，引入一个公共的 js 文件，并实现 jQuery ajax - ajaxSetup() 方法。根据我们的需求，重写 beforeSend、complete、error 等方法，实现业务逻辑判断。

参考代码：
```js
let requestSuccessCode = 200;  // 表示请求成功
let tokenName = "accountToken";
$.ajaxSetup({
    // ajax请求之前进行accountToken封装
    beforeSend: function (xhr) {
        if(accountToken && accountToken != '') {
            xhr.setRequestHeader(tokenName, accountToken);
        }
    },
    // ajax 请求完成返回结果
    complete : function(request) {
        if(request.status == 200) {
            let responseCode = request.responseJSON.code;
            if(responseCode == 6001) {
                clearUserCookie();
                let url = window.location.href;
                redirectLogin(url);
            } else if (responseCode == 6002) {
                window.open("/index.html");
            }
        }
    },
    // 表示请求错误
    error :function(request){
        console.info(request);
    }
});
```
