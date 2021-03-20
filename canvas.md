## 介绍
Canvas API 提供了一个通过JavaScript 和 HTML的<canvas>元素来绘制图形的方式。它可以用于动画、游戏画面、数据可视化、图片编辑以及实时视频处理等方面。

Canvas API主要聚焦于2D图形。而同样使用<canvas>元素的 WebGL API 则用于绘制硬件加速的2D和3D图形。

## 入门

#### 注意事项
1. 可以初始化画布大小(默认w:300px h:150px), 如果css定义的大小和初始画布比例不一致会导致扭曲
   
   ![](F:\Workspace\learn\canvas\note\正常.png)![](F:\Workspace\learn\canvas\note\变形.png)
   
2. 可以在其中放置替换内容，文字 or 图片

3. 必须有结束标签</canvas>, 如果没有，文档其余部分会当成替换内容，从而不显示

#### 渲染上下文
- 通过`getContext()`方法获取渲染上下文和它的绘画功能，它只有一个参数，即上下文的格式，如下例，'2d'
```
const canvas = document.querySelector('#canvas')
const ctx = canvas.getContext('2d')
```

#### 检查支持性
```
if (canvas.getContext) {
    const ctx = canvas.getContext()
    // draw
} 
else {
    // do something
} 
```

#### 一个骨架
尽管script中的内容并不推荐出现在HTML中，这里只是为了简洁
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Canvas</title>
    <style>
        #canvas {
            border: 1px solid #333;
        }
    </style>
</head>
<body>
    <canvas id="canvas" width="300" height="300"></canvas>
    <script>
        function draw() {
            const canvas = document.querySelector('#canvas')
            if (canvas.getContext) {
                const ctx = canvas.getContext('2d')
            }
        }
    </script>
</body>
</html
```

#### 一个简单的例子
效果就是上面扭曲与正常展示中正常那样
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Canvas</title>
    <style>
        #canvas {
            border: 1px solid #333;
        }
    </style>
</head>
<body>
    <canvas id="canvas"  width="200" height="200"></canvas>
    <script>
        (function draw() {
            const canvas = document.querySelector('#canvas')
            if (canvas.getContext) {
                const ctx = canvas.getContext('2d')

                ctx.fillStyle = "rgba(10, 12, 255, 0.5)"
                ctx.fillRect(10, 10, 100, 100)

                ctx.fillStyle = "rgba(255, 255, 11, 0.5)"
                ctx.fillRect(20, 20, 400, 400)
            }
        })()
    </script>
</body>
</html>
```

## 绘制图形

#### 画布栅格
在开始绘制图形之前，我们需要了解一下画布栅格，如下图所示，栅格的起点为左上角即坐标(0, 0)，而栅格中的一个单元相当于canvas中的一像素。所有元素的定位都相对于原点，图中的蓝色正方形左上角的坐标为相对于x轴的距离y像素，相对于y周的距离x像素，即(x, y)
`此处有一幅图片(画布栅格)`

#### 绘制矩形
`canvas`只支持两种形式的图形绘制，`矩形`和`路径`，可以通过多路径生成的方法绘制复杂图形

- 绘制填充矩形`fillRect(x, y, width, height)`，填充矩形的颜色`fillStyle = "color"`
- 绘制描边矩形`strokeRect(x, y, width, height)`， 描边矩形的颜色`strokeStyle = "color"`
- 清除指定矩形区域`clearRect(x, y, width, height)`， 使清除区域完全透明

#### 一个结合起来的矩形例子
```
function draw() {
    const canvas = document.querySelector('#canvas')
    if (canvas.getContext) {
        const ctx = canvas.getContext('2d')

        // 填充矩形及其颜色
        ctx.fillStyle = "red"
        ctx.fillRect(10, 10, 100, 100)

        // 描边矩形及其颜色
        ctx.strokeStyle = "#fff"
        ctx.strokeRect(20, 20, 80, 80)

        // 清除矩形指定区域，使清除区域完全透明
        ctx.clearRect(30, 30, 60, 60)
    }
}
```
它看起来像这样:

![](F:\Workspace\learn\canvas\note\绘制矩形.png)

#### 绘制路径
- `beginPath()`新建一条路径
- `closepath()`闭合路径，对于描边路径来说不会自动闭合，如有需要，则要调用进行路径闭合
- `fill()`填充路径
- `stroke()`描边路径
- `moveTo()`将笔触移动到指定的坐标上。例如，下面三角形的例子中用来指定路径的起始位置
- `lineTo(x, y)`绘制一条直线, 从当前点 到点(x, y)结束。当前点即之前点的结束点，也可以通过`moveTo`指定当前点
- `arc(x, y, radius, startAngle, endAngle, anticlockwise)`, 绘制圆弧或者圆， 以点(x, y)为圆心，以radius为半径，起始弧度为endAngle，结束弧度为endAngle，根据anticlockwise为true还是false决定是逆时针还是顺时针 来画圆(弧)。注：弧度=(Math.PI/180)*角度
- 贝塞尔曲线，下面的图描述了它们的关系，蓝色的为开始点和结束点，橙色的为控制点
  ``此处有一幅图片(贝塞尔曲线关系图)``
  1. `quadraticCurveTo(cp1x, cp1y, x, y)`, 二次贝塞尔曲线, 有一个控制点, (cp1x, cp2y)控制点坐标, (x, y)结束点坐标
  2. `bezierCurveTo(cp1x, cp1y, cp2x, cp2y, x, y)`, 三次贝塞尔曲线, 有两个控制点, (cp1x, cp1y)控制点1的坐标, (cp2x, cp2y)控制点2的坐标, (x, y)结束点的坐标
- 使用路径绘制矩形`rect(x, y, width, height)`, (x, y)为矩形左上角的顶点

  
#### 绘制三角形的例子
蓝色的为填充三角形，黄色的为描边三角形
```
function draw() {
    const canvas = document.querySelector('#canvas')
    if (canvas.getContext) {
        const ctx = canvas.getContext('2d')
        
        // 新建一条路径
        ctx.beginPath()
        // 这里指定路径的起始坐标
        ctx.moveTo(20, 20)
        // 指定一条直线的结束点
        ctx.lineTo(150, 20)
        // 指定另外一条直线的结束点
        ctx.lineTo(20, 150)
        // 填充颜色
        ctx.fillStyle = "skyblue"
        // 填充
        ctx.fill()

        ctx.beginPath()
        ctx.moveTo(180, 20)
        ctx.lineTo(230, 20)
        ctx.lineTo(180, 70)
        // 闭合路径
        ctx.closePath()
        // 描边颜色
        ctx.strokeStyle = "gold"
        // 描边
        ctx.stroke()
    }
}
```
它看起来像这样:
`此处有一幅图片(绘制路径三角形)`

#### 绘制圆弧的例子
这里为了美观就多画了几个
```
function draw() {
    const canvas = document.querySelector('#canvas')
    if (canvas.getContext) {
        const ctx = canvas.getContext('2d')

        // 新建一条路径
        ctx.beginPath()
        // 以(100, 100)为坐标，20像素为半径，起始弧度为0, 结束弧度为(Math.PI/180)*360, 逆时针来绘制一个圆
        ctx.arc(100, 100, 20, 0, (Math.PI/180)*360, true)
        // 填充
        ctx.fill()

        ctx.beginPath()
        // 顺时针
        ctx.arc(150, 100, 20, 0, (Math.PI/180)*360, false)
        // 描边
        ctx.stroke()
        
        ctx.beginPath()
        ctx.arc(200, 100, 20, (Math.PI/180)*170, (Math.PI/180)*270, true)
        ctx.fill()

        ctx.beginPath()
        ctx.arc(100, 150, 20, (Math.PI/180)*170, (Math.PI/180)*270, true)
        ctx.stroke()

        ctx.beginPath()
        ctx.arc(150, 150, 20, (Math.PI/180)*20, (Math.PI/180)*180, false)
        ctx.fill()

        ctx.beginPath()
        ctx.arc(200, 150, 20, (Math.PI/180)*60, (Math.PI/180)*280, false)
        ctx.stroke()
    }
}
```
它看起来像这样:
`此处有一幅图片(绘制路径圆或圆弧)`


#### 绘制二次贝塞尔曲线的例子
控制贝塞尔曲线有一定的难度，因此我们简单的画一下(其实我也不知道画的是啥，凑合看吧😂)
```
function draw() {
    const canvas = document.querySelector('#canvas')
    if (canvas.getContext) {
        const ctx = canvas.getContext('2d')
        ctx.beginPath()
        ctx.moveTo(25,25)
        ctx.quadraticCurveTo(75, 15, 100, 25)
        ctx.quadraticCurveTo(125,50, 100, 75)
        ctx.quadraticCurveTo(75, 100, 25, 75)
        ctx.quadraticCurveTo(10, 50, 25, 25)
        ctx.fill()
    }
}
```
它看起来像这样:
`此处有一幅图片(绘制路径二次贝塞尔曲线)`


#### 绘制三次贝塞尔曲线的例子
这个例子来自MDN，因此看上去还不错
```
function draw() {
    const canvas = document.querySelector('#canvas')
    if (canvas.getContext) {
        const ctx = canvas.getContext('2d')
        ctx.beginPath()
        ctx.moveTo(25,25)
        ctx.bezierCurveTo(75, 37, 70, 25, 50, 25);
        ctx.bezierCurveTo(20, 25, 20, 62.5, 20, 62.5);
        ctx.bezierCurveTo(20, 80, 40, 102, 75, 120);
        ctx.bezierCurveTo(110, 102, 130, 80, 130, 62.5);
        ctx.bezierCurveTo(130, 62.5, 130, 25, 100, 25);
        ctx.bezierCurveTo(85, 25, 75, 37, 75, 40);
        ctx.fillStyle = "red"
        ctx.fill()
    }
}
```
它看起来像这样:
`此处有一幅图片(绘制路径三次贝塞尔曲线)`


#### Path2D对象
上面的所有的路径方法都可以在Path2D中使用
- 空的Path2D对象`new Path2D()`
- 克隆Path2D对象`new Path2D(path)`
- 从SVG建立Path2D对象`new Path2D(d)`, 
  ```
  // 该路径先移动到点(M218, 20), 然后再水平右移60个单位(h, 60), 然后垂直下移动30个单位(v, 30), 然后水平左移30个单位(h, -30), z再回到起点Z
  const p = new Path2D("M218 20 h 60 v 30 h -30 Z")
  ```

#### 一个Path2D对象的例子
```
function draw() {
    /** @type {HTMLCanvasElement} */
    const canvas = document.querySelector('#canvas')
    if (canvas.getContext) {
        const ctx = canvas.getContext('2d')
        const rect = new Path2D()
        rect.rect(20, 20, 50, 50)

        const round = new Path2D()
        round.arc(100, 100, 30, 0, (Math.PI/180)*360, true)

        ctx.fill(rect)
        ctx.stroke(round)
    }
}

```
它看起来像这样:
`此处有一幅图片(绘制路径Path2D对象)`


## 颜色和样式

#### 色彩
支持符合CSS3颜色值标准的有效字符串，如`red`、`#ff0000`、`rgb(255, 0, 0)`、`rgba(255, 0, 0, 0.5)`都表示红色(rgba有透明度)
- 填充颜色`fillStyle = color`
- 描边颜色`strokeStyle = color`
- 全局透明度`globalAlpha = transparencyValue`，范围从0.0-1.0

#### 一个填充和描边透明颜色的例子
```
function draw() {
    const canvas = document.querySelector('#canvas')
    if (canvas.getContext) {
        const ctx = canvas.getContext('2d')
        for (let i=0;i<=9;i++) {
            for(let j=0;j<=9;j++) {
                ctx.beginPath()
                ctx.arc(20+j*25, 20+i*25, 10, 0, Math.PI*2, true)
                if (i%2 == j%2) {
                    ctx.fillStyle = `rgba(${Math.floor(255-20*i)}, ${Math.floor(255-20*j)}, ${Math.floor(0+40*(j+i))}, 0.7)`
                    ctx.fill()
                }
                else {
                    ctx.stroke()
                    ctx.strokeStyle = `rgba(${Math.floor(255-20*i)}, ${Math.floor(255-20*j)}, ${Math.floor(0+40*(j-i))}, 0.9)`
                }
            }
        }
    }
}
```
它看起来像这样:
![](F:\Workspace\learn\canvas\note\色彩.png)

#### 一个全局透明度的例子
```
function draw() {
    const canvas = document.querySelector('#canvas')
    if (canvas.getContext) {
        const ctx = canvas.getContext('2d')
        // 设置全局透明度
        ctx.globalAlpha = 0.4
        const clolor = ['black', 'red', 'orange', 'yellow', 'green', 'cyan', 'blue', 'purple']
        for (let i=0;i<=7;i++) {
            ctx.beginPath()
            ctx.arc(150, 150, 80-i*10, 0, Math.PI*2, true)
            ctx.fillStyle = color[7-i]
            ctx.fill()
        }
    }
}
```
它看起来像这样:
![](F:\Workspace\learn\canvas\note\色彩全局透明度.png)

#### 线型
- 线条宽度`lineWidth = value`，请注意，就像开头说的，我们可以用网格来代表canvas的坐标格，一个网格对应屏幕上的一个像素点，要想绘制出有着清晰边缘的线条，就要确保整条线条的区域要落到像素边缘上，你可以这么理解，即`线条的长和宽是整数的像素`，且`线条左上角的坐标也是个整数坐标`。下面的例子中你会看到有些线条边缘模糊(如果你有仔细看效果图)，正是因为我没经过准确计算造成的。
- 线条端点的样式`lineCap = type`, type有三个值，默认值为 `butt`端点处是正常的，`round`端点处加了直径是线宽的半圆，`square`端点处加了边长是线宽的半个正方形
- 线条与线条间接合处的样式`lineJoin = type`，type有三个值，默认值为 `miter`线条会在连接处外侧延伸交于一点上，`round`边角会被磨圆，`bevel`边角会被沿直线磨掉一个等腰三角
- 设置两条线条相交倾斜的极限长度`miterLimit = value`, 只有在 lineJoin = miter 时才有效，值只有正数才有效，0、负数、NaN、Infinity都会被忽略。当倾斜长度超过该值后，lineJoin的效果会变成bevel
- 设置虚线`setLineDash()` 参数是一个数组，为线段和间隙的交替值。`lineDashOffset`设置起始偏移量


#### 一个设置线条宽度的例子
```
function draw() {
    const canvas = document.querySelector('#canvas')
    if (canvas.getContext) {
        const ctx = canvas.getContext('2d')
        const width = [0.1, 0.2, 0.1, 0.1, 0.4, 0.2, 0.1, 0.3, 0.4, 0.2]
        for (let i=0; i<width.length; i++) {
            // 设置线宽
            ctx.lineWidth = 1 + width[i]*10
            ctx.beginPath()
            ctx.moveTo(20+i*8, 20)
            ctx.lineTo(20+i*8, 100)
            ctx.stroke()
        }
    }
}
```
它看起来像这样:
![](F:\Workspace\learn\canvas\note\线形线宽.png)

#### 一个设置线段端点样式的例子
```
function draw() {
    const canvas = document.querySelector('#canvas')
    if (canvas.getContext) {
        const ctx = canvas.getContext('2d')
        ctx.strokeStyle = 'red'
        ctx.beginPath()
        ctx.moveTo(20, 20)
        ctx.lineTo(200, 20)
        ctx.stroke()
        ctx.beginPath()
        ctx.moveTo(20, 150)
        ctx.lineTo(200, 150)
        ctx.stroke()

        ctx.strokeStyle = 'black'
        const types = ['butt', 'round', 'square']
        types.forEach((type,i) => {
            ctx.lineWidth = 20
            ctx.lineCap = type
            ctx.beginPath()
            ctx.moveTo(50+i*50, 20)
            ctx.lineTo(50+i*50, 150)
            ctx.stroke()
        })
    }
}
```
它看起来像这样:
![](F:\Workspace\learn\canvas\note\线形端点样式.png)

#### 一个设置线条接合处样式的例子
```
function draw() {
            /** @type {HTMLCanvasElement} */
            const canvas = document.querySelector('#canvas')
            if (canvas.getContext) {
                const ctx = canvas.getContext('2d')
                ctx.strokeStyle = 'red'
                ctx.beginPath()
                ctx.moveTo(-5, 120)
                ctx.lineTo(420, 120)
                ctx.stroke()

                ctx.strokeStyle = 'black'
                ctx.lineWidth = 20

                // 线条结合点默认值miter
                // ctx.lineJoin = 'miter'
                ctx.beginPath()
                ctx.moveTo(10, 20)
                ctx.lineTo(50, 120)
                ctx.lineTo(100, 20)
                ctx.stroke()

                ctx.lineJoin = 'round'
                ctx.beginPath()
                ctx.moveTo(150, 20)
                ctx.lineTo(200, 120)
                ctx.lineTo(250, 20)
                ctx.stroke()

                ctx.lineJoin = 'bevel'
                ctx.beginPath()
                ctx.moveTo(300, 20)
                ctx.lineTo(350, 120)
                ctx.lineTo(400, 20)
                ctx.stroke()
            }
        }
```
它看起来像这样:
![](F:\Workspace\learn\canvas\note\线型线条接合样式.png)

#### 一个设置虚线的例子
```
function draw() {
    const canvas = document.querySelector('#canvas')
    if (canvas.getContext) {
        const ctx = canvas.getContext('2d')
        ctx.clearRect(0, 0, canvas.width, canvas.height)
        ctx.setLineDash([4, 2])
        ctx.lineDashOffset = 2
        ctx.strokeRect(10, 10, 200, 100)
    }
}
```
它看起来像这样:
![](F:\Workspace\learn\canvas\note\设置虚线.png)

#### 渐变
- 线性渐变`createLinearGradient(x1, y1, x2, y2)`, 渐变起点(x1, y1), 渐变终点(x2, y2)
- 径向渐变`createRadialGradient(x1, y1, r1, x2, y2, r2)`, 前三个参数，定义一个以(x1, y1)为圆心,r1为半径的圆, 后三个参数，定义另一个以(x2, y2)为圆心，r2为半径的圆
- 添加色标`addColorStop(position, color)`给渐变对象上色, position表示渐变中颜色所在的相对位置，范围为0.0-1.0，color是一个有效的css颜色值

#### 一个渐变的例子
```
function draw() {
    const canvas = document.querySelector('#canvas')
    if (canvas.getContext) {
        const ctx = canvas.getContext('2d')
        const lineargradient =  ctx.createLinearGradient(0, 0, 100, 100)
        lineargradient.addColorStop(0, '#f00')
        lineargradient.addColorStop(0.5, '#ff4')
        lineargradient.addColorStop(0.7, '#fff')
        lineargradient.addColorStop(1, '#0ff')
        ctx.fillStyle = lineargradient
        ctx.fillRect(10, 10, 100, 100)

        const lineargradient2 =  ctx.createLinearGradient(150, 0, 250, 100)
        lineargradient2.addColorStop(0, '#f00')
        lineargradient2.addColorStop(0.5, '#ff4')
        lineargradient2.addColorStop(0.7, '#fff')
        lineargradient2.addColorStop(1, '#0ff')
        ctx.strokeStyle = lineargradient2
        ctx.lineWidth = 5
        ctx.strokeRect(150, 10, 100, 100)

        const radialgradient = ctx.createRadialGradient(50, 200, 10, 50, 200, 50)
        radialgradient.addColorStop(0, '#0f4')
        radialgradient.addColorStop(0.5, '#f00')
        radialgradient.addColorStop(1, '#fff')
        ctx.fillStyle = radialgradient
        ctx.fillRect(0, 150, 100, 100)
    }
}
```
它看起来像这样:
![](F:\Workspace\learn\canvas\note\渐变.png)

#### 图案样式
- `createPattern(image, type)`, image可以是一个图像对象的引用，也可以是一个canvas对象，type的参数分为`repeat`,`repeat-x`,`repeat-y`,`no-repeat`

#### 一个图案样式的例子
```
function draw() {
    const canvas = document.querySelector('#canvas')
    if (canvas.getContext) {
        const ctx = canvas.getContext('2d')
        const img = new Image()
        img.src = '../note/log.png'
        img.onload = function() {
            const pat = ctx.createPattern(img, 'repeat')
            ctx.fillStyle = pat
            ctx.fillRect(1, 0, 273, 270)
        }
    }
}
```
它看起来像这样:
![](F:\Workspace\learn\canvas\note\图案样式.png)

#### 阴影
- `shadowOffsetX = value`, `shadowOffsetY = value`, 负值往左或上延伸, 正值往右或下延伸
- `shadowBlur = value`,设定阴影模糊程度，默认值0
- `shadowColor = color`, 设定阴影颜色

#### 一个阴影的例子
```
function draw() {
    const canvas = document.querySelector('#canvas')
    if (canvas.getContext) {
        const ctx = canvas.getContext('2d')
        ctx.shadowOffsetX = -15
        ctx.shadowOffsetY = -15
        ctx.shadowBlur = 2
        ctx.shadowColor = 'rgba(0, 0, 0, 0.3)'
        
        ctx.font = '50px Microsoft YaHei'
        ctx.fillStyle = 'rgba(0, 100, 255, 0.7)'
        ctx.fillText('星海昔织', 20, 100)
    }
}
```
它看起来像这样:
![](F:\Workspace\learn\canvas\note\阴影.png)


#### 
1
```

```
它看起来像这样:
![](F:\Workspace\learn\canvas\note\.png)