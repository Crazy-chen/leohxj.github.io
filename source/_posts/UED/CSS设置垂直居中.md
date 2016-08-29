title: CSS设置垂直居中
date: 2013-8-16 14:13:21
categories: [UED]
tags: [CSS]
---

让元素垂直居中有很多方法，这里挑选几个。<!--more-->


## Negative Margins ##
最常见的做法，给margin-top, margin-left设置负值，然后通过绝对定位设置top, left 为50%。

固定宽高的情况下，浏览器缩小到一定尺寸时候，容器会跑出屏幕外。而设置高度height: auto的话，可以自适应撑满容器。

html:

```html
<body>
    <div class="Center-Container Clear">
        <div class="Center-Block is-Negative">
          <h4 class="Title">Absolute Center,<br> Negative Margins.</h4>
          <p>This box is absolutely centered vertically within its container using negative margins.</p>
      </div>
  </div>
</body>
```

css:

```css
.is-Negative {
        width: 300px;
        height: 200px;
        padding: 20px;
        position: absolute;
        top: 50%; left: 50%;
        margin-left: -170px; /* (width + padding)/2 */
        margin-top: -120px; /* (height + padding)/2 */
        background: #619271;
}
```

宽高设置百分比的情况下，如果里面有文字，需要设置overflow来隐藏(hidden)或出现滚动条(auto)。

html:

```html
<body>
    <div class="Center-Container Clear">
        <div class="Center-Block is-Negative">
          <h4 class="Title">Absolute Center,<br> Negative Margins.</h4>
          <p>This box is absolutely centered vertically within its container using negative margins.</p>
      </div>
  </div>
</body>
```

css:

```css
.is-Negative {
        width: 30%;
        height: 20%;
        padding: 2%;
        position: absolute;
        top: 50%; left: 50%;
        margin-left: -16%; /* (width + padding)/2 */
        margin-top: -11%; /* (height + padding)/2 */
        background: #619271;
        overflow: auto; /* hidden */
}
```

### 优点 ###
- 兼容性好
- 代码少

### 缺点 ###
- 固定宽度的时候，不可以设置min-/max-
- 内容不能自适应撑满容器


## table-cell ##
需要设置容器包裹，兼容性较好。

html:

```html
<body>
    <div class="Center-Container is-Table Clear">
        <div class="Table-Cell">
          <div class="Center-Block">
              <h4>Header 44444</h4>
              <p>This is header border</p>
          </div>
      </div>
  </div>
</body>
```

css:

```css
.Center-Container.is-Table {
    color: #4fa46b;
    width: 100%;
    height: 100%;
    clear: both;
    display: table;
}
.is-Table .Table-Cell {
  display: table-cell;
  vertical-align: middle;
}
.is-Table .Center-Block {
  width: 50%;
  height: auto;
  margin: 0 auto;
  background: #2e5f3e;
}
```

### 优点 ###
- 高度可以自动变化
- 兼容性好

### 缺点 ###
- 需要额外的标签包裹


## inline-block ##
感觉最高端的写法，美观大气！

html:

```html
<body>
    <div class="Center-Container is-Inline">
      <div class="Center-Block">
        <p id="more">Discover more you can do it with xxxxxxxx</p>
      </div>
  </div>
</body>
```

css:

```css
.Center-Container.is-Inline {
    text-align: center;
    width: 100%;
    height: 100%;
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    font-size: 0;
}


.Center-Container.is-Inline:after {
    text-align: center;
    content: '';
    display: inline-block;
    height: 100%;
    vertical-align: middle;
    margin-right: -.25em
}

.is-Inline .Center-Block {
  display: inline-block;
  vertical-align: middle;
  width: 90%;
  background: #2E5F3E;
}
#more {
    font-size: 32px;
}
```

### 优点 ###
- 标签结构层次少
- 兼容性强, IE7都可以

### 缺点 ###
- 最外层需要设置`font-size:0`，里面元素需要重新定义`font-size`


## 参考资料 ##
- [绝对居中神文](http://codepen.io/shshaw/full/gEiDt)

