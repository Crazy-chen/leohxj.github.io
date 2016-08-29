title: Transition Vs Animation
date: 2013-7-30 16:56:30
categories: [UED]
tags: [前端开发, 动画, CSS3]
---
通过CSS展示动画有两种方式，分别是translation和animation。我一直很好奇两者的区别，所以有必要研究一下。
<!--more-->

先来看一些例子：[25 CSS3 Transitions and Animations Effects Tutorials](http://inspiretrends.com/css3-transitions-and-animations-effects-tutorials/)。

## Transitions ##
先看一下兼容性，移动平台支持较好。Android 2.1+, iOS 3.2+， 但是IE方面，只支持IE 10。如何使用，先看代码：

```css
#id-name {
    -webkit-transition: all 1s ease-in-out;
    -moz-transition: all 1s ease-in-out;
    -o-transition: all 1s ease-in-out;
    transition: all 1s ease-in-out;
}
```

需要注意的是前缀问题，这里推荐一个插件，[prefixfree](http://leaverou.github.io/prefixfree/)。

### transition-property ###
`transition-property`是要进行过渡变化的属性，可以设置多个值，逗号分隔。

### transition-duration ###
`transition-duration`是元素属性过渡的时间，可以设置多个值，逗号分隔。

### transition-duration ###
`transition-delay`是元素延迟进行过渡的时间，可以设置多个值，逗号分隔。

### transition-timing-function ###
`transition-timing-function`是缓动函数，默认是`ease`，可以设置多个值，逗号分隔。


## Animation ##
依然先看下兼容性，移动平台上，Android4.0+才完全支持。IE同样只支持IE10。

个人感觉animation使用更加灵活，可以定义`keyframes`较为方便。例如:

```css
@keyframes resize {
  0% {
    padding: 0;
  }
  50% {
    padding: 0 20px;
    background-color:rgba(255,0,0,0.2);
  }
  100% {
    padding: 0 100px;
    background-color:rgba(255,0,0,0.9);
  }
}

#box {
  animation-name: resize;
  animation-duration: 1s;
  animation-iteration-count: 4;
  animation-direction: alternate;
  animation-timing-function: ease-in-out;
}
```

### animation-name ###
标明`keyframe`名称。

### animation-duration ###
动画执行时间。

### animation-delay ###
动画延迟执行时间。

### animation-timing-function ###
缓动函数，可以写在keyframe中，是不同阶段应用不同缓动。

### animation-iteration-count ###
循环播放次数。

### animation-direction ###
播放顺序，执行完一次之后从0%还是100%播放。

### animation-fill-mode ###
执行完animation之后停留的画面，是0%还是100%。

### animation-play-state ###
初始播放状态。


## 总结 ##
`transition`的兼容性更优一点。配合hover效果，会有完整的过渡。

`animation`可以定义关键帧，动画控制属性更多。

## 参考资料 ##
- [CSS3 Transitions, Transforms, Animation, Filters and more!](http://css3.bradshawenterprises.com/)
