一，托管自己的域名到cf  
阿里云腾讯云随便买个域名（假设为woaixx.cf），添加域名到cf，根据提示修改dns，等待托管成功  

二，创建workers  
自定义名称，假设为tg，修改xxx为自己的tg机器人token数字部分，保存并部署
```
const whitelist = ["/botxxxxxxxxxx:"];
const tg_host = "api.telegram.org";
addEventListener('fetch', event => {
    event.respondWith(handleRequest(event.request))
})
function validate(path) {
    for (var i = 0; i < whitelist.length; i++) {
        if (path.startsWith(whitelist[i]))
            return true;
    }
    return false;
}
async function handleRequest(request) {
    var u = new URL(request.url);
    u.host = tg_host;
    if (!validate(u.pathname))
        return new Response('Unauthorized', {
            status: 403
        });
    var req = new Request(u, {
        method: request.method,
        headers: request.headers,
        body: request.body
    });
    const result = await fetch(req);
    return result;
}
```

三，绑定域名  
1.新增dns解析  
在cf中首页已经托管的域名中新增一个DNS解析，类型   A   ，名称自定义（这里假设为tg，即子域名为tg.woaixx.cf ，IPv4 随便填，最重要的是开启代理（一朵小黄云）  

2.关联域名  
进入域名首页，侧边栏workers，添加路由，路由填写上一步的子域名 + `/*`，  
比如我刚刚DNS解析的域名是tg.woaixx.cf，那在路由一栏则填写   `tg.woaixx.cf/*` ，  
服务选择刚创建的Worker（tg），环境选择production，必须先创建Worker再来关联域名，表示通过这个自定义的域名来访问Worker服务  

3.使用域名  
直接使用`https://tg.woaixx.cf`代理tg机器人


