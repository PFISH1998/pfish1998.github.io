---
title: Python爬虫应对网页JavaScirpt混淆/加密
date: 2019-03-02 14:50:39
tags: Python
index_img: http://cdn.pfish.xyz/pic/20190302/XdFeTC30lFeh.png
banner_img: http://cdn.pfish.xyz/pic/20190302/Mbv8boBESUFO.png
---
最近在处理一个登录爬虫时，遇到了在网页端运用JavaScript混淆和对用户名密码加密的手段，但不是很复杂。
<!--more-->

# 从何说起

缘由是在准备用 Python requests 进行模拟登录一个网页，惯例通过抓包发现网页表单往后端 Post 输入的用户名和密码的时候进行了加密；

我想大概率还是开发人员针对安全考虑，不像某些网页直接往后端明文发送一些敏感数据。但这也对爬虫登录带来了小小的麻烦。

接下来的操作就是找到负责这一部分功能的代码，然后要么试着模拟代码用方便的语言实现相同的操作，要么是直接把该代码下载下来，用Python相关的工具运行。

#### 找到它

在HTML代码中和抓包记录中找到该 JS 函数


发现，这跟平常的 JS 不一样，这就涉及到一个叫做**JavaScript 加密/混淆**的概念：

### JavaScript 混淆/加密

**混淆：经过编码将变量和函数原命名改为毫无意义的命名（如function(a,b,c,e,g)等），以防止他人窥视和窃取 Javascript 源代码，也有一定压缩效果。**

**加密：一般用eval方法加密，效果与混淆相似，也做到了压缩的效果。**

**压缩：删除 Javascript 代码中所有注释、跳格符号、换行符号及无用的空格，从而压缩 JS 文件大小，优化页面加载速度。**


 ```JavaScript
// 被加密混淆后的 JavaScript 代码：
 eval(function(p, a, c, k, e, d) {
    e = function(c) {
        return (c < a ? "" : e(parseInt(c / a))) + ((c = c % a) > 35 ? String.fromCharCode(c + 29) : c.toString(36))
    }
    ;
    if (!''.replace(/^/, String)) {
        while (c--)
            d[e(c)] = k[c] || e(c);
        k = [function(e) {
            return d[e]
        }
        ];
        e = function() {
            return '\\w+'
        }
        ;
        c = 1;
    }
    ;while (c--)
        if (k[c])
            p = p.replace(new RegExp('\\b' + e(c) + '\\b','g'), k[c]);
    return p;
}('b 9="o+/=";p q(a){b e="";b 8,5,7="";b f,g,c,1="";b i=0;m{8=a.h(i++);5=a.h(i++);7=a.h(i++);f=8>>2;g=((8&3)<<4)|(5>>4);c=((5&s)<<2)|(7>>6);1=7&t;k(j(5)){c=1=l}v k(j(7)){1=l}e=e+9.d(f)+9.d(g)+9.d(c)+9.d(1);8=5=7="";f=g=c=1=""}u(i<a.n);r e}', 32, 32, '|enc4||||chr2||chr3|chr1|keyStr|input|var|enc3|charAt|output|enc1|enc2|charCodeAt||isNaN|if|64|do|length|ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789|function|encodeInp|return|15|63|while|else'.split('|'), 0, {}))

 ```

 看起来真是一团乱，不知道的以为是这程序员在炫技...

 至于怎么还原（解密），现在有很多工具类的网站都能够做到：
 http://www.bm8.com.cn/jsConfusion/


 ```JavaScript
 // 经过反混淆后的 JavaScript 代码

 var keyStr = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/=";

function encodeInp(input) {
    var output = "";
    var chr1, chr2, chr3 = "";
    var enc1, enc2, enc3, enc4 = "";
    var i = 0;
    do {
        chr1 = input.charCodeAt(i++);
        chr2 = input.charCodeAt(i++);
        chr3 = input.charCodeAt(i++);
        enc1 = chr1 >> 2;
        enc2 = ((chr1 & 3) << 4) | (chr2 >> 4);
        enc3 = ((chr2 & 15) << 2) | (chr3 >> 6);
        enc4 = chr3 & 63;
        if (isNaN(chr2)) {
            enc3 = enc4 = 64
        } else if (isNaN(chr3)) {
            enc4 = 64
        }
        output = output + keyStr.charAt(enc1) + keyStr.charAt(enc2) + keyStr.charAt(enc3) + keyStr.charAt(enc4);
        chr1 = chr2 = chr3 = "";
        enc1 = enc2 = enc3 = enc4 = ""
    } while (i < input.length);
    return output
}
 
 ```
 可以看到原来的 JS 代码是对字符进行了一些操作，前端加密，后端再解密。你可以用其他语言实现这个过程（高手，真正的高手），但最简单最方便最没风险，的还是直接拿来用。

 不然出了偏差（

### Python 运行 JavaScript 代码

得到了原始的 JS 代码，那自然也就能利用它来加密数据，让后端服务器照样认为这是个从网页发来的正常请求。

用 Python 来模拟运行 JavaScrip 的库有几个，这里用 **PyExecJS** 实现。
1. 首先需要 `pip install PyExcJS` 安装；
2. 确保机器中有 JavaScript 运行环境（没有的话安装一个 Node.js）

检查一下：
 ```Python
 import execjs
 print(execjs.get().name)
 ```

 我安装的是 Node.js 所以显示：
  ```shell
>>> import execjs
>>> print(execjs.get().name)
Node.js (V8)
  ```

一切准备就绪，那么就试试能不能达到这个效果。

把要执行的 JS 代码保存成后缀为 .js 的文件

Python 模拟运行 JavaScript 代码的代码：
 ```Python
 import execjs

 node = execjs.get()
 file = 'encode.js'
 ctx = node.compile(open(file).read())
 # 原 JS 函数名 为 encodeInp()
 user = 'encodeInp("{}")'.format(username)
 pwd = 'encodeInp("{}")'.format(password)
 # 网页向后端 POST 的格式
 result = '{}%%%{}'.format(str(ctx.eval(user)), str(ctx.eval(password)
 ```

先通过 execjs.get() 方法声明一个运行环境，然后通过compile()方法来执行刚才保存的函数，接着构造一个字符串传递参数，最后eval()方法模拟执行返回结果。

有了这个加密后的数据，就能直接用 requests 发送 POST 请求了：

 `response = requests.post(url, data={"encoded": result})`

 但一般登录后如果还要进行页面跳转选择之类的操作的话，还得用 requests.Session() 方法

  ```Python
  import requests
  session = requests.Session()
  session.get(index_url)
  response = session.post(login_url, data={"encoded":result})
  if response.status_code:
    ...  
  ```


