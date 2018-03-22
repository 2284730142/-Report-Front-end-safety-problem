# X-Frame-Options响应头

**一、简介**

X-Frame-Options HTTP 响应头是用来给浏览器指示允许一个页面可否在 `<frame>`, `<iframe>` 或者 `<object>` 中展现的标记。网站可以使用此功能，来确保自己网站的内容没有被嵌到别人的网站中去，也从而避免了点击劫持 (clickjacking) 的攻击。

**二、使用(需要在服务器中配置)**

X-Frame-Options 有三个值:
    
    a.DENY
    表示该页面不允许在 frame 中展示，即便是在相同域名的页面中嵌套也不允许。
    
    b.SAMEORIGIN
    表示该页面可以在相同域名页面的 frame 中展示。
    
    c.ALLOW-FROM uri
    表示该页面可以在指定来源的 frame 中展示。
    
换一句话说，如果设置为 DENY，不光在别人的网站 frame 嵌入时会无法加载，在同域名页面中同样会无法加载。另一方面，如果设置为 SAMEORIGIN，那么页面就可以在同域名页面的 frame 中嵌套。