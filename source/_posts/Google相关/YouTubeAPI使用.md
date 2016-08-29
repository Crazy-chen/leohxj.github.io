title: YouTubeAPI使用
date: 2013-10-6 22:22:09
categories: google相关
tags: [google, YouTube]
---

最近的一个项目里用到了通过JavaScript加载YouTube视频和为YouTube的视频添加赞和评论的功能，自己也看了好多天API，值得总结下。<!--more-->

## 加载视频
目前YouTube的HTML5版视频还只是Beta阶段，需要用户自己去开启，地址是[http://www.youtube.com/html5](http://www.youtube.com/html5)。外部加载视频的方式是通过iframe，DEMO如下:

```html
<!DOCTYPE html>
<html>
  <body>
    <!-- 1. The <iframe> (and video player) will replace this <div> tag. -->
    <div id="player"></div>

    <script>
      // 2. This code loads the IFrame Player API code asynchronously.
      var tag = document.createElement('script');

      tag.src = "https://www.youtube.com/iframe_api";
      var firstScriptTag = document.getElementsByTagName('script')[0];
      firstScriptTag.parentNode.insertBefore(tag, firstScriptTag);

      // 3. This function creates an <iframe> (and YouTube player)
      //    after the API code downloads.
      var player;
      function onYouTubeIframeAPIReady() {
        player = new YT.Player('player', {
          height: '390',
          width: '640',
          videoId: 'M7lc1UVf-VE',
          playerVars: {
              'autohide': 1,
              'showinfo': 0,
              'modestbranding': 0,
              'rel': 0
          },
          events: {
            'onReady': onPlayerReady,
            'onStateChange': onPlayerStateChange
          }
        });
      }

      // 4. The API will call this function when the video player is ready.
      function onPlayerReady(event) {
        event.target.playVideo();
      }

      // 5. The API calls this function when the player's state changes.
      //    The function indicates that when playing a video (state=1),
      //    the player should play for six seconds and then stop.
      var done = false;
      function onPlayerStateChange(event) {
        if (event.data == YT.PlayerState.PLAYING && !done) {
          setTimeout(stopVideo, 6000);
          done = true;
        }
      }
      function stopVideo() {
        player.stopVideo();
      }
    </script>
  </body>
</html>
```

这里需要注意的是构造函数`YT.player()`,第一个参数是需要转换为iframe的id，第二个参数是一个JSON对象，里面包括了:

- width, 视频宽度
- height， 视频高度
- videoId， 视频的ID号，youtube视频链接里有
- playerVars，构造的详细参数，链接见参考
- events， 视频的事件，见 [https://developers.google.com/youtube/iframe_api_reference#Events](https://developers.google.com/youtube/iframe_api_reference#Events)

经过这样的操作，视频就可以加载到我们的网页之后，并且受我们的控制了。

## OAuth 2.0 授权
这是google的授权方式，通过OAth2.0才可以使用API。那么我们就需要通过JavaScript来获取授权。这里需要使用到[google JavaScript API](https://developers.google.com/api-client-library/javascript/start/start-js)。

在使用API获取授权之前，我们还需要在[https://code.google.com/apis/console](https://code.google.com/apis/console)注册我们的应用，获取一个`client_id`和`API key`。这一块内容可以参考[OAuth 2.0 Authorization](https://developers.google.com/youtube/2.0/developers_guide_protocol_oauth2)。注册我们的应用的时候，需要注意的是，`Redirect URIs`可以写成`http://www.clientapprove.com/preview/LB_Pokemon_YouTube_Gadget/test/oauth2callback`，就是你的网站地址加上`/oauth2callback`(也可以不加)。`JavaScript origins`填写你的domain，需要注意的是，如果你的网站支持https协议，那么写成`https://www.clientapprove.com`，不支持的请写`http://www.clientapprove.com`。不然无法完成授权。

完成授权之后，我们可以通过JavaScript来使用API，DEMO如下(为youtube授权)：

```javascript
var clientId = '530271286664.apps.googleusercontent.com',
  apiKey = 'AIzaSyBePCO42fiIk2NfoZUBpgJxPScAwXCiVd4',
  scopes = 'https://www.googleapis.com/auth/youtube';

function handleClientLoad() {
  gapi.client.setApiKey(apiKey);
  window.setTimeout(checkAuth, 1);
}

function handleAuthClick(event) {
  gapi.auth.authorize({
    client_id: clientId,
    scope: scopes,
    immediate: false
  }, handleAuthResult);
  return false;
}

function checkAuth() {
  gapi.auth.authorize({
    client_id: clientId,
    scope: scopes,
    immediate: true
  }, handleAuthResult);
}

function handleAuthResult(authResult) {
  var authorizeButton = document.getElementById('authorize-button');
  if (authResult && !authResult.error) {
    authorizeButton.style.visibility = 'hidden';
    makeApiCall();
  } else {
    authorizeButton.style.visibility = '';
    authorizeButton.onclick = handleAuthClick;
  }
}


function makeApiCall() {
  console.log('It\'s OK');
}
```
需要注意的是`<script src="https://apis.google.com/js/client.js?onload=handleClientLoad"></script>`必须插入到上面一段脚本的下面。授权完成之后，就可以获取到用户的`access_token`等一系列操作了。

## 添加comment
YouTube的API现在有v3,v2两个版本，但是comment功能只有v2实现，并且传递的数据是XML格式。

想要完成评论功能，还需要在[https://code.google.com/apis/youtube/dashboard](https://code.google.com/apis/youtube/dashboard)申请一个`developer key`。

下面是通过jQuery的ajax操作demo:

```javascript
function addComment() {
  // 注意，要设置accessToken还有X-GData-Key
  var accessToken = gapi.auth.getToken().access_token;
  console.log('accessToken: ', accessToken);
    var xmlData = '<?xml version="1.0" encoding="UTF-8"?> <entry xmlns="http://www.w3.org/2005/Atom"xmlns:yt="http://gdata.youtube.com/schemas/2007"> <content>This is a crazy video. test 444</content> </entry>';
    var data = $.parseXML(xmlData);
    $.ajax({
      headers: {
        'Authorization': 'Bearer ' + accessToken,
        'GData-Version': 2,
        'X-GData-Key': 'key=AI39si6M3GXB3A8ZNWA6GFcWb_D6uskJL4iluYwMBZaSE6hwW45laNC4LnPqS-gsPqH1joxGfVtQYs8Ry4TobJcPJS3JawIXXA'
      },
      type: 'POST',
      url: 'https://gdata.youtube.com/feeds/api/videos/0DfPkpFjH_g/comments',
      contentType: 'application/atom+xml',
      dataType: 'xml',
      processData: false,
      data: data,
      success: function(data) {
        console.log(data);
      }
    });
}
```
需要注意的地方是通过`POST`方式并且ajax中需要指定`headers`信息，url链接地址是`https://gdata.youtube.com/feeds/api/videos/VIDEO_ID/comments`。


## 添加like/dislike
和comment不同的是url地址变换成了`https://gdata.youtube.com/feeds/api/videos/0DfPkpFjH_g/ratings`, DEMO如下：

```javascript
function addLike() {
  var accessToken = gapi.auth.getToken().access_token;
  console.log('accessToken: ', accessToken);
    var xmlData = '<?xml version="1.0" encoding="UTF-8"?> <entry xmlns="http://www.w3.org/2005/Atom"xmlns:yt="http://gdata.youtube.com/schemas/2007"> <yt:rating value="like"/> </entry>';
    var data = $.parseXML(xmlData);
    $.ajax({
      headers: {
        'Authorization': 'Bearer ' + accessToken,
        'GData-Version': 2,
        'X-GData-Key': 'key=AI39si6M3GXB3A8ZNWA6GFcWb_D6uskJL4iluYwMBZaSE6hwW45laNC4LnPqS-gsPqH1joxGfVtQYs8Ry4TobJcPJS3JawIXXA'
      },
      type: 'POST',
      url: 'https://gdata.youtube.com/feeds/api/videos/0DfPkpFjH_g/ratings',
      contentType: 'application/atom+xml',
      dataType: 'xml',
      processData: false,
      data: data,
      success: function(data) {
        console.log(data);
      }
    });
}

function addDislike() {
  var accessToken = gapi.auth.getToken().access_token;
  console.log('accessToken: ', accessToken);
    var xmlData = '<?xml version="1.0" encoding="UTF-8"?> <entry xmlns="http://www.w3.org/2005/Atom"xmlns:yt="http://gdata.youtube.com/schemas/2007"> <yt:rating value="dislike"/> </entry>';
    var data = $.parseXML(xmlData);
    $.ajax({
      headers: {
        'Authorization': 'Bearer ' + accessToken,
        'GData-Version': 2,
        'X-GData-Key': 'key=AI39si6M3GXB3A8ZNWA6GFcWb_D6uskJL4iluYwMBZaSE6hwW45laNC4LnPqS-gsPqH1joxGfVtQYs8Ry4TobJcPJS3JawIXXA'
      },
      type: 'POST',
      url: 'https://gdata.youtube.com/feeds/api/videos/0DfPkpFjH_g/ratings',
      contentType: 'application/atom+xml',
      dataType: 'xml',
      processData: false,
      data: data,
      success: function(data) {
        console.log(data);
      }
    });
}
```

## 一些参考链接
- [iframe_api](https://developers.google.com/youtube/iframe_api_reference)
- [OAuth 2.0 Authorization](https://developers.google.com/youtube/2.0/developers_guide_protocol_oauth2)
- [Community Features](https://developers.google.com/youtube/2.0/developers_guide_protocol_ratings)