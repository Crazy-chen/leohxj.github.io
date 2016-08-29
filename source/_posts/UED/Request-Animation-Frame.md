title: Request Animation Frame
date: 2013-8-8 14:16:37
categories: [UED]
tags: [动画, HTML5]
---

这是新的HTML5 API，提供了更加顺滑的动画操作。
<!--more-->

## 示例 ##
HTML:

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <link rel="stylesheet" href="style.css">
<body>
    <div id="my-element">Click to start</div>
    <script src="app.js"></script>
</body>
</html>
```

CSS:

```css
#my-element {
  position: absolute;
  left: 0;
  width: 200px;
  height: 200px;
  padding: 1em;
  background: tomato;
  color: #FFF;
  font-size: 2em;
  text-align: center;
}
```

JS:

```javascript
(function() {
  var lastTime = 0;
  var vendors = ['ms', 'moz', 'webkit', 'o'];
  for (var x = 0; x < vendors.length && !window.requestAnimationFrame; ++x) {
    window.requestAnimationFrame = window[vendors[x] + 'RequestAnimationFrame'];
    window.cancelAnimationFrame = window[vendors[x] + 'CancelAnimationFrame'] || window[vendors[x] + 'CancelRequestAnimationFrame'];
  }

  if (!window.requestAnimationFrame) window.requestAnimationFrame = function(callback, element) {
    var currTime = new Date().getTime();
    var timeToCall = Math.max(0, 16 - (currTime - lastTime));
    var id = window.setTimeout(function() {
      callback(currTime + timeToCall);
    }, timeToCall);
    lastTime = currTime + timeToCall;
    return id;
  };

  if (!window.cancelAnimationFrame) window.cancelAnimationFrame = function(id) {
    clearTimeout(id);
  };
}());

window.addEventListener('load', function() {

  var elem = document.getElementById('my-element'),
    startTime = null,
    endPos = 500,
    // in pixels
    duration = 2000; // in milliseconds

  function render(time) {
    if (time === undefined) {
      time = new Date().getTime();
    }
    if (startTime === null) {
      startTime = time;
    }

    elem.style.left = ((time - startTime) / duration * endPos % endPos) + 'px';
  }

  elem.addEventListener('click', function() {
    (function animationLoop() {
      render();
      requestAnimationFrame(animationLoop, elem);
    })();
  }, false);

}, false);
```