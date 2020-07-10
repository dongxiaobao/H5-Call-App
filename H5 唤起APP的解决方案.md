### H5 唤起APP的解决方案

#### IOS

##### 1.url scheme

这个方案基本上就是针对微信、qq内置浏览器，qq浏览器等之外的其余浏览器，从native那边要一个scheme ，然后放在a标签里或者location.href跳一下就行了

用一个iframe去做的一个跳页，有的话唤起scheme没有的话，会触发定时器跳到下载地址。但是这个方式在ios里面，在没有app的时候会遇到两次提示，

```js

 var openApp = function (src) {
        // 通过iframe的方式试图打开APP，如果能正常打开，会直接切换到APP，并自动阻止a标签的默认行为
        // 否则打开a标签的href链接
        const ifr = document.createElement('iframe');
        ifr.src = src;
        ifr.style.display = 'none';
        document.body.appendChild(ifr);
        var poenTime = +new Date()
        window.setTimeout(() => {
          document.body.removeChild(ifr);
          if ((+new Date()-openTime>2500)){
            window.location = 'APP Store下载的地址 '
          }
        }, 20);
      };

```

##### 2.Universal Link（ios）

 这是iOS9推出的一项功能，如果你的应用支持Universal Links(通用链接)，那么就能够方便的通过传统的HTTP链接来启动APP(如果iOS设备上已经安装了你的app，不需要额外做任何判断等)，或者打开网页(iOS设备上没有安装你的app)。或许可以更简单点来说明，在iOS9之前，对于从各种从浏览器，Safari、UIWebView或者 WKWebView中唤醒APP的需求，我们通常只能使用scheme。

```js
window.location.href ="native端战友给的Universal Link"
```

##### 总结

兼容写法

```
if (isGreaterThan9){
   window.location.href ="native端给的Universal Link" ;
   return;
}
openApp(src)
```

#### android

方法类似

```
 if (openApp('url scheme url')) {
            openApp('url scheme url');
          } else {
            setTimeout(() => {
              window.location.href = 'APP 市场下载地址';
            }, 600);
          }
      }
```



