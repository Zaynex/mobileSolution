# 移动端解决方案

> 在公司实习的时候接触到一些移动端上面的坑，记录之。

## 唤起APP

在页面中唤起APP可以使用 scheme 。
一般在下载页面，点击下载时如果用户已经安装了APP那么就直接打开APP，否则就跳转到下载页。但是由于`微信`内不允许唤起scheme，通过添加应用宝的下载地址即可。
1. 检测是否为微信
```
function downLoad(){
        var  redirectUrl = 'http://note.youdao.com/collect/index.html',
            isAndroid = !!navigator.userAgent.match(/android/ig),
            android_yingyongbao = 'http://a.app.qq.com/o/simple.jsp?pkgname=com.youdao.note&ckey=CK1362580791055';
            if(isAndroid) {
                isWeixin ? holdApp(android_scheme, android_yingyongbao, redirectUrl) : holdApp(android_scheme, androidUrl, redirectUrl);
            } else {
                holdApp(ios_scheme, iosLoadUrl, redirectUrl);                
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

## 视频解决方案
在微信中不支持自动播放，需要通过添加事件`$(".playSrc").play()`来达到播放的效果。
```html
 <video class="playSrc"
webkit-playsinline="true"
playsinline="true"
preload="auto"
x5-video-player-fullscreen="false"
muted
poster="assets/imgs/video.jpg"
type="videp/mp4"
loop>
您的浏览器不支持播放器，请通过右侧文字步骤进行操作。
</video>
```

## 原生HTML页面检测APP版本
```
**@isNewBuild 是否为大于等于baseBuild的新版本
**@baseBuild 检测版本
**
var nowBuild = navigator.userAgent.toString().split(' '),
    isNewBuild = false,
    // 版本号
    baseBuild = '5.9.7.0';
    for(var i = 0, l = nowBuild.length; i < l; i++) {
            // 这里检测的是IOS版本，后续如果使用android版本，在indexOf中替换
        if(~nowBuild[i].indexOf('YnoteiOS')) {
            isNewBuild = nowBuild[i].split('/')[1] >= baseBuild;
        }
    };
```

## 移动端布局
- rem
- @media
- flex
- meta viewport 

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
