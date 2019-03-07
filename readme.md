### 类似material-ui的button效果

### 步骤

#### 1. 在页面写出一个`button` 并赋予简单的样式
 > 代码
 ```html
 <body>
    <div>
        <button>这是一个按钮</button>
    </div>
 </body>
 ```

 ```css
 button {
    display: block; /* button 默认是inline-block 无法用margin给它居中 */
    margin: 50px auto;
    height: 60px;
    width: 150px;
    color: #FFF;
    font-size: 16px;
    background: #E95546;
    outline: none;
    border: 0;
    cursor: pointer; /* 为了增加用户体验 */

    position: relative; /* 为了保持和ripple的位置关系 */
    overflow: hidden; /* 为了遮盖超出的ripple */
 }
  ```
  > 效果

![](https://user-gold-cdn.xitu.io/2019/3/7/16957e790ed5c0c3?w=1000&h=422&f=png&s=18610)

#### 2. 添加水波纹的基础css样式
> 代码
```css
.ripple {
  position: absolute; /* 为了保持和button的位置关系 */
  border-radius: 50% 50%; /* 水波纹圆形 */
  background: rgba(255, 255, 255, 0.4);  /* 水波纹颜色 */

  /* 下面与动画效果相关 是0%时候的状态 */
  /* 默认的 transform-origin 是 center 即中心点 */
  transform: scale(0);
  -webkit-transform: scale(0);
  opacity: 1;
}
```

#### 3. 给水波纹加上动画的css
> 代码
```css
.rippleEffect {
  /* 执行时长0.6s、效果为渐变(linear)、名称为rippleDrop的动画 */
  -webkit-animation: rippleDrop .6s linear;
  animation: rippleDrop .6s linear;
}

@keyframes rippleDrop {
  /* 下面是动画100%时候的状态 */
  100% {
    transform: scale(2);
    -webkit-transform: scale(2);
    opacity: 0;
  }
}

@-webkit-keyframes rippleDrop {
  100% {
    transform: scale(2);
    -webkit-transform: scale(2);
    opacity: 0;
  }
}
```

#### 4. 最后添加点击事件
> 代码

```javascript
$("button").click(function (e) {
  $(".ripple").remove(); // 每次先把之前添加的水波纹div删除

  let button_left = $(this).offset().left; // button距离页面左边的距离
  let button_top = $(this).offset().top; // button距离页面上边的距离
  let button_width = $(this).width(); // button的宽度
  let button_height = $(this).height(); // button的高度

  // 水波纹div为一个圆形，使得它的半径等于button的最长边，故在这里计算最长边是多少
  let ripple_width = 0;
  ripple_width = button_width > button_height ? button_width : button_height;

  // e.pageX是click事件的鼠标触发点距离页面左边的距离
  // e.pageY是click事件的鼠标触发点距离页面上边的距离
  // 这里的算法是，算出鼠标点击点的坐标为 (e.pageX - button_left, e.pageY - button_top)
  // 但是由于`transform-origin`默认是center，所以这里再减去半径才是div应该的位置
  let ripple_x = e.pageX - button_left - ripple_width / 2;
  let ripple_y = e.pageY - button_top - ripple_width / 2;

  // 往button里面塞水波纹div
  $(this).prepend("<div class='ripple'></div>");

  // 给水波纹div应用样式和动画
  $(".ripple")
  .css({
    width: ripple_width + 'px',
    height: ripple_width + 'px',
    top: ripple_y + 'px',
    left: ripple_x + 'px'
  })
  .addClass("rippleEffect");

})
```
> **Attention** 别忘了引入`jquery`哦
```
<script src="https://cdn.bootcss.com/jquery/2.2.3/jquery.min.js"></script>
```
> 效果

![](https://user-gold-cdn.xitu.io/2019/3/7/16957fef51d03373?w=952&h=402&f=gif&s=652803)

### END

> [源码地址](https://github.com/Sotyoyo/jq-copy-material-buttion)