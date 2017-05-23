# 移动端解决方案

> 这里是个人在公司实习的时候接触到的东西，为避免忘记以及深入学习，决定将零散的知识点整理成文档。

## 唤起APP

在页面中唤起APP可以使用 scheme 。
一般在下载页面，点击下载时如果用户已经安装了APP那么就直接打开APP，否则就跳转到下载页。但是由于`微信`内不允许唤起scheme，通过添加应用宝的下载地址即可。
1. 检测是否为微信
```
function downLoad(){
        var  redirectUrl = 'http://note.youdao.com/collect/index.html',
            android_yingyongbao = 'http://a.app.qq.com/o/simple.jsp?pkgname=com.youdao.note&ckey=CK1362580791055';
        if(type === '1'){
            if(isAndroid) {
                isWeixin ? holdApp(android_scheme, android_yingyongbao, redirectUrl) : holdApp(android_scheme, androidUrl, redirectUrl);
            } else {
                holdApp(ios_scheme, iosLoadUrl, redirectUrl);                
            }
        } else if(type === '0'){
            if(clickKeyfrom !== 'wechat') {
                redirectUrl = insideTips && insideTips[clickKeyfrom] && insideTips[clickKeyfrom].link; 
                window.location = redirectUrl + '?time=' + ((new Date()).getTime());
            } else {
                var wechatScheme = 'weixin://',
                    wechatLoad = 'http://weixin.qq.com/cgi-bin/readtemplate?t=w_down';
                holdApp(wechatScheme, wechatLoad, redirectUrl);
            }
        }
    }

    function holdApp(scheme, loadUrl, successUrl, onFail, onSuccess){
         var last = Date.now(),
                doc = window.document,
                ifr = doc.createElement('iframe');
                //创建一个隐藏的iframe
        ifr.src = scheme;
        ifr.style.cssText = 'display:none;';
        doc.body.appendChild(ifr);
        setTimeout(function() {
            doc.body.removeChild(ifr);
            //setTimeout回小于2000一般为唤起失败 
            if (Date.now() - last < 2000) {
                if (typeof onFail == 'function') {
                    onFail();
                } else {
                    window.location = loadUrl;
                }
            } else {
                if (typeof onSuccess == 'function') {
                    onSuccess();
                } else {
                    window.location = successUrl
                }
            }
        }, 1000);
    }
```

## 客户端平台检测

## 视频解决方案

## 布局

## 活动页开发

在滑动翻页的时候，如果页面有元素监听了 `toucheend` 事件，那么翻页滑动的时候碰到该元素就会触发事件，使用 `click`即可避免。

推荐滚动插件(不依赖任何库)
- swiper

## 其他
### IOS微信双击屏幕上滑bug
```
(function()
{
    var agent = navigator.userAgent.toLowerCase();        //检测是否是ios
    var iLastTouch = null;                                //缓存上一次tap的时间
    if (agent.indexOf('iphone') >= 0 || agent.indexOf('ipad') >= 0)
    {
        document.body.addEventListener('touchend', function(event)
        {
            var iNow = new Date()
                .getTime();
            iLastTouch = iLastTouch || iNow + 1 /** 第一次时将iLastTouch设为当前时间+1 */ ;
            var delta = iNow - iLastTouch;
            if (delta < 500 && delta > 0)
            {
                event.preventDefault();
                return false;
            }
            iLastTouch = iNow;
        }, false);
    }
})();
```
