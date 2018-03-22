# -Report-Front-end-safety-problem
前端安全问题及其处理方式的简书

一、前端安全问题(附带的[1]是某个东西的具体说明)

1.XSS(Cross Site Scripting，跨站脚本攻击)<a href="./specification/CSP.md">[1]</a>

    原因： a.浏览器错误的将攻击者提供的用户输入数据当做JavaScript脚本给执行了。
    
    防御措施： a.各类数据进行编码。
              b.设置CSP HTTP Header[1]。
              c.输入验证。
              d.开启浏览器XSS防御。
              
2.iframe的风险

    原因： a.前端页面需要用到第三方提供的页面组件，通常会以iframe的方式引入。
          b.域名过期被恶意攻击者抢注，或是第三方被黑客攻击，导致iframe内容替换。
         
    防御措施： a.H5中，给iframe添加sandbox属性。
    
3.点击劫持<a href="./specification/X-Frame-Options.md">[2]</a>

    原因： a.攻击者诱导用户点击其准备的iframe的内容。
    
    防御措施： a.FrameBreaking。
              b.X-Frame-Options：DENY 的HTTP Header[2]。
     
4.错误内容推断

    原因： a.攻击者恶意上传例如伪装成图片的js脚本，同时该脚本在服务器上存活了下来，然后受害者进行浏览并点击此脚本时，浏览器对响应的实际内容进行推断，最终脚本执行。
    
    防御措施： a.X-Content-Type-Options HTTP Header禁止浏览器去推断响应类型。
    
5.第三方插件的安全性

    原因： a.第三方代码会有安全漏洞。
    
    防御措施： a.自动化工具进行检查。
    
6.用https也会出现坑

    原因： a.黑客可以利用SSL Stripping这种攻击手段，强制让HTTPS降级回HTTP，从而继续进行中间人攻击。
    
    防御措施： a.HSTS（HTTP Strict Transport Security，强制性安全措施）
    
7.本地存储数据泄露

    原因： a.在前端存储任何敏感、机密的数据，都会面临泄露的风险，就算是在前端通过JS脚本对数据进行加密基本也无济于事。
    
    防御措施： a.尽可能不在前端存这些数据。
    
8.缺乏静态资源完整性校验

    原因： a.前端应用将一些静态资源放到CDN（Content Delivery Networks）中，攻击者如果对CDN中资源进行污染，那么我们就会拿到有问题的资源。
    
    防御措施： a.SRI（Subresource Integrity，子资源完整性）。
