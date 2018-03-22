#  HTTP Strict Transport Security(HSTS)

**一、简介**

HSTS(HTTP Strict Transport Security)是国际互联网工程组织IETF发布的一种互联网安全策略机制。采用HSTS策略的网站将保证浏览器始终连接到该网站的HTTPS加密版本，不需要用户手动在URL地址栏中输入加密地址，以减少会话劫持风险。

**二、描述**

一个网站接受一个HTTP的请求，然后跳转到HTTPS，用户可能在开始跳转前，通过没有加密的方式和服务器对话，比如，用户输入http://foo.com或者直接foo.com。这样存在中间人攻击潜在威胁，跳转过程可能被恶意网站利用来直接接触用户信息，而不是原来的加密信息。网站通过HTTP Strict Transport Security通知浏览器，这个网站禁止使用HTTP方式加载，浏览器应该自动把所有尝试使用HTTP的请求自动替换为HTTPS请求。

**三、格式**

HSTS响应头格式 Strict-Transport-Security: max-age=expireTime [; includeSubDomains] [; preload]

    a.max-age，单位是秒，用来告诉浏览器在指定时间内，这个网站必须通过HTTPS协议来访问，也就是对于这个网站的HTTP地址，浏览器需要先在本地替换为HTTPS之后再发送请求。
    
    b.includeSubDomains，可选参数，如果指定这个参数，表明这个网站所有子域名也必须通过HTTPS协议访问。
    
    c.preload，可选参数，一个浏览器内置的使用HTTPS的域名列表。
    
**四、缺点**

HSTS并不是HTTP会话劫持的完美解决方案。用户首次访问某网站是不受HSTS保护的。这是因为首次访问时，浏览器还未收到HSTS，所以仍有可能通过明文HTTP来访问。如果用户通过HTTP访问HSTS保护的网站时，以下几种情况存在降级劫持可能：

    a.以前从未访问过该网站
    
    b.最近重新安装了其操作系统
    
    c.最近重新安装了其浏览器
    
    d.切换到新的浏览器
    
    e.切换到一个新的设备，如：移动电话
    
    f.删除浏览器的缓存
    
    g.最近没访问过该站并且max-age过期了
    
解决这个问题目前有两种方案:

    方案一
    
    在浏览器预置HSTS域名列表，就是HSTS Preload List方案。该域名列表被分发和硬编码到主流的Web浏览器。客户端访问此列表中的域名将主动的使用HTTPS，并拒绝使用HTTP访问该站点。
    
    方案二
    
    将HSTS信息加入到域名系统记录中。但这需要保证DNS的安全性，也就是需要部署域名系统安全扩展。
    
其它可能存在的问题:由于HSTS会在一定时间后失效(有效期由max-age指定)，所以浏览器是否强制HSTS策略取决于当前系统时间。大部分操作系统经常通过网络时间协议更新系统时间，如Ubuntu每次连接网络时，OS X Lion每隔9分钟会自动连接时间服务器。攻击者可以通过伪造NTP信息，设置错误时间来绕过HSTS。解决方法是认证NTP信息，或者禁止NTP大幅度增减时间。比如：Windows 8每7天更新一次时间，并且要求每次NTP设置的时间与当前时间不得超过15小时。