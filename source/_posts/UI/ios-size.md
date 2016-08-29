title: ios界面尺寸
date: 2014-10-13 13:55:14
categories: [UI]
tags: [UI]
---

主要介绍下ios中safari的尺寸。ios界面主要被分割为如下几部分:

- Status bar
- Navigation bar
- Content
- Tab bar

如图：
![Sizes of iPhone UI Elements](http://www.idev101.com/code/User_Interface/img/bothPhones.jpg)
<!--more-->

## 设备尺寸
Resolutions & Display Specifications:

| Devices          | point      |   pixel               | Physical Device |
|------------------|:----------:|----------------------:|----------------:|
| iPhone 2g/3g/3gs | 320x480    | 320x640               | 3.5''           |
| iPhone 4/4s      | 320x480    | 640x960               | 3.5''           |
| iPhone 5/5s      | 320x568    | 640x1136              | 4''             |
| iPhone 6         | 375x667    | 750x1334              | 4.7''           |
| iPhone 6 Plus    | 414x736    | 1242x2208(1080x1920)  | 5.5''           |
| iPad 1/2         | 768x1024   | 768x1024              | 9.7''           |
| iPad 3/4/air     | 768x1024   | 1536x2048             | 9.7''           |
| iPad Mini        | 768x1024   | 768x1024              | 7.9''           |
| iPad Mini 2      | 768x1024   | 1536x2048             | 7.9''           |


Commonly used design elements of Safari (point):

| Devices               | Height of Status Bar | Height of Nav Bar(protrait)/when scroll | Nav Bar when scroll(landscape)/when scroll | Height of Tab Bar(portrait)/when scroll | Height of Tab Bar(landscape)/when scroll | landscape fullview (no status bar) |
|-----------------------|:--------------------:|:---------------------------------------:|:------------------------------------------:|:-------------------------------------------:|:----------------------------------------:|-----------------------------------:|
| ios5/iPhone           | 20px                 | 60px/0px                                | 60px/0px                                   | 44px/44px               | 32px/32px                              | YES                                |
| ios5/iPad             | 20px                 | 60px/60px                               | 60px/60px                                  | 44px/44px               | 44px/44px                              | NO                                 |
| ios6/iPhone           | 20px                 | 60px/0px                                | 60px/0px                                   | 44px/44px               | 32px/32px                              | YES                                |
| ios6/iPad             | 20px                 | 60px/60px                               | 60px/60px                                  | 44px/44px               | 44px/44px                              | NO                                 |
| ios7/iPhone           | 20px                 | 44px/20px                               | 44px/0px                                   | 44px/0px                     | 44px/0px                               | YES                                |
| ios7/iPad             | 20px                 | 44px/44px                               | 44px/44px                                  | 32px/32px               | 32px/32px                              | NO                                 |


### 尺寸说明
1. iphone下，ios5，6竖屏滚动，上面的地址栏会消失，但是下面的tab bar不会消失。而在ios7下，竖屏滚动，上面的地址栏会变小，下面的tab bar会消失。
2. iphone下, 横屏都可以全屏，ios5,6需要手动点击，ios7自动全屏。全屏状态下，ios5,6 status可以消失。ios7下status不会消失。
3. ipad下，不可以全屏，且status bar + nav bar + tab bar固定大小。大小就是status bar + nav bar + tab bar的高度。
4. ios7下，页面如果设置100%高，实际高度还是可以滚动，需要强制样式上写入!important.


## 参考资料
- [iPhone 6 Screens Demystified](http://www.paintcodeapp.com/news/iphone-6-screens-demystified)
- [The iOS Design Cheat Sheet ](http://ivomynttinen.com/blog/the-ios-7-design-cheat-sheet/)
- [iOS 7 UI Transition Guide](https://developer.apple.com/Library/ios/documentation/UserExperience/Conceptual/TransitionGuide/Bars.html)
- [iPhone 5 Display Size and Web Design Tips](http://www.kylejlarson.com/blog/2012/iphone-5-web-design/)
