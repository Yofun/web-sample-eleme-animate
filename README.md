# 前言
每次吃饭点外卖的时候（暴露了自己是个死肥宅，手动滑稽），或者在淘宝购物的时候，将商品加入购物车时会有一个很炫酷的动画，如下图饿了么点餐动画：
![饿了么原图](https://upload-images.jianshu.io/upload_images/4179198-fd5a42bae04ef00a.gif?imageMogr2/auto-orient/strip)
所以百度了一下前端使用css实现这个效果，然后就自己就照葫芦画瓢的写了一个小小的demo，完全当作学习了一把。

# 界面
在界面上，我是参考了图片上的界面，写的css，然后在美团上面扒的数据。完成后的界面如下图：
![界面UI](https://upload-images.jianshu.io/upload_images/4179198-9f38eb5b53a1430c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

开始想着只是做一个简单的实现效果，界面上只有div框，按钮什么的这些，反正想着很丑的界面，但是自己看到饿了么这么好看的界面，自己就忍不住模仿着写了一下下。
主要用到的是移动端适配方案：flexible.js + rem的方案

# 动画实现
![](https://upload-images.jianshu.io/upload_images/4179198-b77ba9c57b7b9e63.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
1. 用户在点击图片中`标注1`的时候，创建一个div，插入到body中，改div暂且叫`bar`吧
2. 设置`bar`的css样式为
    ```
    {
      position: absolute;
      top: 点击位置X坐标;
      left: 点击位置Y坐标;
      width: 0.533333rem;
      height: 0.533333rem;
      border-radius: 50%;
      background: #02b6fd;
      transition: left .6s linear, top .6s cubic-bezier(0.5, -0.5, 1, 1)
    }
    ```
    其中，`点击位置的X坐标`和`点击位置的Y坐标`是通过点击时js获取到的
3. 然后设置最终位移的位置到`标注2`的位置

# 代码实现
首先获取到页面上所有按钮，然后给按钮添加点击事件，获取到点击当前按钮的位置坐标信息
```
    var buttons = document.getElementsByClassName('ele_main_goods_add')
    for (let i = 0; i < buttons.length; i++) {
        buttons[i].onclick = function () {
            let x = event.pageX - this.offsetWidth / 2;
            let y = event.pageY - this.offsetWidth / 2;
            // 方案一
            // createBall01(x, y)
            // 方案二
            // createBall02(x, y)
        }
    }
```
**方案一：使用定位+transition的方式实现**
```
    function createBall01(left, top) {
        let bar = document.createElement('div');
        bar.style.position = 'absolute'
        bar.style.left = (left) + 'px'
        bar.style.top = (top) + 'px'
        bar.style.width = '0.533333rem'
        bar.style.height = '0.533333rem'
        bar.style.borderRadius = '50%'
        bar.style.backgroundColor = '#02b6fd'
        bar.style.transition = 'left .6s linear, top .6s cubic-bezier(0.5, -0.5, 1, 1)'

        document.body.appendChild(bar)
        // 添加动画属性
        setTimeout(() => {
            let target = document.querySelector('.ele_car_icon')
            bar.style.left = (target.offsetLeft + target.offsetWidth / 2) + 'px'
            bar.style.top = (target.offsetTop) + 'px'
        }, 0);

        /**
         * 动画结束后，删除自己
         */
        bar.ontransitionend = function () {
            this.remove()
        }
    }
```

**方案二：使用transform + transition的方式实现**
```
    function createBall02(x, y) {
        const bar = document.createElement('div')
        bar.style.position = 'absolute'
        bar.style.left = '0'
        bar.style.top = '0'
        bar.style.width = '0.533333rem'
        bar.style.height = '0.533333rem'
        bar.style.borderRadius = '50%'
        bar.style.backgroundColor = '#02b6fd'
        // transform
        bar.style.transform = 'translate(' + x + 'px,' + y + 'px)'
        bar.style.transition = 'transform .5s linear'


        document.body.appendChild(bar)
        // 添加动画属性
        setTimeout(() => {
            let target = document.querySelector('.ele_car_icon')
            let targetX = (target.offsetLeft + target.offsetWidth / 2)
            let targetY = (target.offsetTop)
            bar.style.transform = 'translate(' + targetX + 'px,' + targetY + 'px)'
        }, 0);

        /**
         * 动画结束后，删除自己
         */
        bar.ontransitionend = function () {
            this.remove()
        }

    }
```


# 最终效果
![实现效果](https://upload-images.jianshu.io/upload_images/4179198-e79c3e1f230ea89c.gif?imageMogr2/auto-orient/strip)

其实还可以使用css的animate来实现，只不过由于需要动态计算目标位置的坐标，实现起来比较困难，所以我就没有写，如果目标位置的坐标值固定，那么可以通过animate改变top、left或者translateX、translateY的值来实现这个效果，垂直方向的动画使用贝塞尔曲线的`function-timing`。


# 参考资料

[初识CSS抛物线动画](https://mickey0524.github.io/2017/08/06/cubic-bezier/)


# 源码

[https://github.com/HyFun/CSS-Sample-ElemeAnimate](https://github.com/HyFun/CSS-Sample-ElemeAnimate)
