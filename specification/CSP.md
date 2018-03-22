# CSP 策略指令

**一、简介**

在内容安全策略 (CSP) 中，网站管理员可以定义多种策略范围。这些策略可以被任意组合来满足需求；您并不需要指定全部策略。CSP 的实质就是白名单制度，开发者明确告诉客户端，哪些外部资源可以加载和执行，等同于提供白名单。它的实现和执行全部由浏览器完成，开发者只需提供配置。CSP 大大增强了网页的安全性。攻击者即使发现了漏洞，也没法注入脚本，除非还控制了一台列入了白名单的可信主机。

**二、启用方式**

两种方法可以启用 CSP:

1.通过 HTTP 头信息的Content-Security-Policy的字段。

2.通过网页的`<meta>`标签。(`<meta http-equiv="Content-Security-Policy" content="script-src 'self'; object-src 'none'; style-src cdn.example.org third-party.org; child-src https:">`)


在上面mata标签中，CSP 做了如下配置：
    
    a.脚本：只信任当前域名
    
    b.<object>标签：不信任任何URL，即不加载任何资源
    
    c.样式表：只信任cdn.example.org和third-party.org
    
    d.框架（frame）：必须使用HTTPS协议加载
    
    e.其他资源：没有限制
    
**三、限制选项**

1.资源加载限制
    
    a.script-src：外部脚本
    
    b.style-src：样式表
    
    c.img-src：图像
    
    d.media-src：媒体文件（音频和视频）
    
    e.font-src：字体文件
    
    f.object-src：插件（比如 Flash）
    
    g.child-src：框架
    
    h.frame-ancestors：嵌入的外部资源（比如<frame>、<iframe>、<embed>和<applet>）
    
    i.connect-src：HTTP 连接（通过 XHR、WebSockets、EventSource等）
    
    j.worker-src：worker脚本
    
    k.manifest-src：manifest 文件 
    
2.default-src设置上面各个选项的默认值。

如果同时设置某个单项限制（比如font-src）和default-src，前者会覆盖后者，即字体文件会采用font-src的值，其他资源依然采用default-src的值。

3.URL 限制

    a.frame-ancestors：限制嵌入框架的网页
    
    b.base-uri：限制<base#href>
    
    c.form-action：限制<form#action>  

4.其他限制
    
    a.block-all-mixed-content：HTTPS 网页不得加载 HTTP 资源（浏览器已经默认开启）
    
    b.upgrade-insecure-requests：自动将网页上所有加载外部资源的 HTTP 链接换成 HTTPS 协议
    
    c.plugin-types：限制可以使用的插件格式
    
    d.sandbox：浏览器行为的限制，比如不能有弹出窗口等。
    
    
5.report-uri        

有时，我们不仅希望防止 XSS，还希望记录此类行为。report-uri就用来告诉浏览器，应该把注入行为报告给哪个网址。

**四、选项值**

每个限制选项可以设置以下几种值，这些值就构成了白名单。多个值也可以并列，用空格分隔。如果同一个限制选项使用多次，只有第一次会生效。如果不设置某个限制选项，就是默认允许任何值。

    a.主机名：example.org，https://example.com:443
    
    b.路径名：example.org/resources/js/
    
    c.通配符：*.example.org，*://*.example.com:*（表示任意协议、任意子域名、任意端口）
    
    d.协议名：https:、data:
    
    e.关键字'self'：当前域名，需要加引号
    
    f.关键字'none'：禁止加载任何外部资源，需要加引号
    
**五、注意点**

1.script-src和object-src是必设的，除非设置了default-src。因为攻击者只要能注入脚本，其他限制都可以规避。而object-src必设是因为 Flash 里面可以执行外部脚本。

2.script-src不能使用unsafe-inline关键字（除非伴随一个nonce值），也不能允许设置data:URL。

3.必须特别注意 JSONP 的回调函数。