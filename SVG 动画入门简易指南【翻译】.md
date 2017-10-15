# SVG 路径动画简易指南

> 原文链接：[https://www.toptal.com/front-end/svg-animation-guide](https://www.toptal.com/front-end/svg-animation-guide)
>
> 作者：[JUAN CALOU](https://www.toptal.com/resume/juan-calou)

任何有开发经验的前端工程师都会考虑到不成体系的设备生态所带来的挑战。设备间不同的屏幕尺寸、分辨率和比例使得产品难以提供一致的体验，对于那些对产品有着像素级完美追求的人这种体验差异尤其显著！
SVG（可缩放的矢量图形）完美地解决了上文中提到的部分问题。尽管 SVG 有它的局限性，但是在某些场景下是非常有用的，如果你有一个好的设计团队，你也可以基于SVG创建一些震撼的视觉体验，而不必担心给浏览器带来过重的渲染负担或阻碍页面的加载时间。

![](http://7xr868.com1.z0.glb.clouddn.com/kjl-toptal-blog-image-1501855758851-cead6880a7875388174043632a50fe07.jpg)

在过去的几个月里，我一直在做一个大量使用了 SVG 及其动画效果的项目。在本文中，我将介绍如何使用SVG及其动画技术为你的 Web 前端开发带来一些新鲜的体验。

### 可缩放矢量图形
SVG 是一种基于 XML 文档的图片格式，所以大部分情况其特性表现类似于 HTML。它为许多常见的几何图形定义了不同的元素，通过组合这些不同的形状元素可以生成 SVG 二维图形。
[SVG 规范](https://www.w3.org/TR/SVG/)是由万维网联盟（W3C）于1999制定的标准，所有的主流浏览器目前均支持了 [SVG 的渲染](https://caniuse.com/#search=svg)。由于 SVG 图形是 XML 文档，因此 Web 浏览器提供的 DOM 相关的 API，同样可作用于与 SVG 图形的交互。
### SVG 路径
如果要说出 SVG 中最强大的元素，毫无疑问是 **<path>** （路径元素）。
路径元素是一个可以构建出你所能想象的几乎任何高级的2D图形的基本形状。路径元素通过一系列绘图命令来生效，它非常类似于1967年的 Logo 编程语言，不同的是它只是更现代化的，为复杂花哨的图形而设计的。这些绘图命令如下图所示，被写在路径元素的  **d** 属性中 ：

```html
<!-- A right-angled triangle -->
<path d="M10 10 L75 10 L75 75 Z" />
```

你可以把它想象成一支虚拟的画笔在屏幕上作画，而路径元素的 d 属性中的绘图命令控制着画笔的走向。上图的示例中，画笔一开始移动到起点坐标 `(10, 10)` （M10 10），以 `(75, 10)` 为终点画直线（L75 10），接着又画一条直线至 `(75, 75)` （L75 75），最后的 `Z` 表示画笔回到起点坐标以闭合路径。

使用一些其他的绘图命令，例如绘圆弧（**A**）、二次贝塞尔曲线（**Q**）、三次贝塞尔曲线（**C**）等等，你可以在 SVG 中创建出很多组合的形状和矢量图形。

你可以点击这里了解更多关于路径元素的知识 >> [path element](http://tutorials.jenkov.com/svg/path-element.html)。

### SVG 路径与 CSS

也许你会问：“好吧我知道 Paths 很强大，但是我怎样才能对它做路径动画呢？”。

因此我们第一步需要利用到 SVG 的两个属性：`stroke-dasharray` 和 `stroke-dashoffset` 。

![](http://7xr868.com1.z0.glb.clouddn.com/kjl-toptal-blog-image-1501855771209-e7241a8962fee8e1e344faa248cade72.jpg)

[stroke-dasharray](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/stroke-dasharray) 属性可以控制图案描边路径的样式，如果你并不想用连续的直线去绘制路径，而希望通过一些不同样式的虚线，你就可以使用这个属性。

由于 SVG 图形其实也是浏览器 DOM 的组成部分，因此 stroke-dasharray 作为一个控制外观的属性，也可以直接用作一个 CSS 样式属性，达到同样的设置效果。

类似的，[stroke-dashoffset](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/stroke-dashoffset) 属性（虚线在原路径下的偏移量）也同样可以使用 CSS 来进行设置。

这两个属性的组合使用可以生成 SVG 路径动画，给人一种图案的轮廓线逐渐拟合的视觉感受。

例如下面这个二次贝塞尔曲线的例子：

![](http://7xr868.com1.z0.glb.clouddn.com/kjl-beisaier.jpg)

```html
<path fill="transparent" stroke="#000000" stroke-width="5" d="M10 80 Q 77.5 10, 145 80 T 280 80" class="path"></path>
```

为了平稳流畅地绘制出这个路径，首先我们需要通过 stroke-dasharray 属性设置虚线段的长度，将 stroke-dasharray 属性的值设为该路径的长度。这样的话就将虚线的每一段 dash 和 gap 的长度等于整段路径的长。

下一步我们需要使用 stroke-dashoffset  属性将虚线的偏移量设置为 0，此时我们看到的路径描边就是没有间断的连续曲线（实际上看到的是虚线段的第一段，前面已经设置每一虚线段的长度等于该曲线的长）。通过设置虚线偏移量等于曲线的长度，那该曲线恰好“消失”（实际上看到的是虚线段的一段间隙）。

通过 stroke-dashoffset 属性，同时结合 CSS3 的 animation，你可以让该曲线一点点的出现在屏幕上，这就是 SVG 路径动画的原理。 

```html
<svg width="300px" height="175px" version="1.1">
    <path fill="transparent" stroke="#000000" stroke-width="4" d="M10 80 Q 77.5 10, 145 80 T 280 80" class="path"></path>
</svg>
```

```scss
svg {
  width: 300px;
  display: block;
  position: absolute;
  
  .path {
    stroke-dasharray: 320;
    stroke-dashoffset: 0;
    animation: dash 1s linear;
  }
  
  @keyframes dash {
    from {
      stroke-dashoffset: 320;
    }
    to {
      stroke-dashoffset: 0;
    }
  }
}
```

> 你可以点击[这里](https://codepen.io/toptalblog/pen/MvjWEz)查看 CODEPEN 上的 [DEMO](https://codepen.io/toptalblog/pen/MvjWEz)。

可以看到，我们只是改变了虚线的偏移来让虚线段的部分一点一点地出现。

运用相同的原理来设置多条路径的动画，可以得到更炫酷的效果：

```html
<svg width="300px" height="175px" version="1.1">
    <path fill="transparent" stroke="#000000" stroke-width="4" d="M10 80 Q 77.5 10, 145 80 T 280 80" class="path"></path>
  <path fill="transparent" stroke="#FF2851" stroke-width="4" d="M10 80 Q 77.5 10, 145 80 T 280 80" class="line2"></path>
  <path fill="transparent" stroke="#000000" stroke-width="4" d="M10 80 Q 77.5 10, 145 80 T 280 80" class="line1"></path>
</svg>
```

```css
svg {
  width: 300px;
  display: block;
  position: absolute;
}
svg .line1 {
  stroke-dasharray: 340;
  stroke-dashoffset: 40;
  animation: dash 1.5s linear alternate infinite;
}
svg .line2 {
  stroke-dasharray: 320;
  stroke-dashoffset: 320;
  animation: dash2 1.5s linear alternate infinite;
}
@keyframes dash {
  from {
    stroke-dashoffset: 360;
  }
  to {
    stroke-dashoffset: 40;
  }
}
@keyframes dash2 {
  from {
    stroke-dashoffset: 280;
  }
  to {
    stroke-dashoffset: -40;
  }
}
```

> 你可以点击[这里](https://codepen.io/toptalblog/pen/eEgPqW)查看 CODEPEN 上的 [DEMO](https://codepen.io/toptalblog/pen/eEgPqW)。

上面在 SVG 中画了3条路径：其中一条是固定的黑色曲线， 有一条沿着路径移动的红色曲线，后面跟着另一条黑色曲线。

stroke-dasharray 和 stroke-dashoffset 是创造大量 SVG 路径动画所要用到的两个重要属性，你可以点击[这里](stroke-dashoffset )（一个方便的小工具）来体会这两个属性。

### 沿 SVG 路径的动画对象

通过 SVG 和 CSS，我们可以让一个对象或者元素沿着 SVG 路径做一些动效，过程中我们会用到两个属性：

- [offset-path](https://developer.mozilla.org/en-US/docs/Web/CSS/offset-path)：offset-path 是一个 CSS 属性，它表示元素的运动路径； 
- [offset-distance](https://developer.mozilla.org/en-US/docs/Web/CSS/offset-distance)：同样是一个 CSS 属性，定义了元素在路径上运动的距离，单位是数值或百分比；


通过组合使用这两个属性，你可以非常容易地创建出类似下面的动画：

```html
<svg width="300px" height="175px" version="1.1">
    <path fill="transparent" stroke="#888888" stroke-width="1" d="M10 80 Q 77.5 10, 145 80 T 280 80" class="path"></path>
</svg>

<div class="ball"></div>
```

```css
svg {
  width: 300px;
  display: block;
  position: absolute;
}

.ball {
  width: 10px;
  height: 10px;
  background-color: red;
  border-radius: 50%;
  
  offset-path: path('M10 80 Q 77.5 10, 145 80 T 280 80');
  offset-distance: 0%;

  animation: red-ball 2s linear alternate infinite;
}

@keyframes red-ball {
  from {
    offset-distance: 0%;
  }
  to {
    offset-distance: 100%;
  }
}
```

>你可以点击[这里](https://codepen.io/toptalblog/pen/qXRJeY)查看 CODEPEN 上的 [DEMO](https://codepen.io/toptalblog/pen/qXRJeY)。

上面我们让一个 `div.ball` 的元素动了起来，同样的我们可以对任何元素（`div` , `image` , `text` ...）做这种路径动画。在这个例子中我们简单的用 offset-path 画出了元素的运动路径，然后用 offset-distance 控制元素在路径上的运动距离从 0% 到100%。

### 使用 JavaScript 做 SVG 动画

以上如果还不足以满足你的动画需求，你可以考虑借助 JavaScript。

使用 JavaScript 对 SVG 元素做动画与对 DOM 元素做动画相似。然而我们可以更容易地实现上面提到的动画效果。之前，我们需要将路径长度硬编码在 CSS 中。借助 JavaScript 的 `path.getTotalLength()` 函数可以获取 DOM 上路径的长度，你可以点击[这里](https://jakearchibald.com/2013/animated-line-drawing-svg/)了解更多。除此之外，有很多第三方库可以帮助你十分容易地制作 SVG 动画。

[Snap.svg](http://snapsvg.io/) 不仅可以使 JavaScript  绘制 SVG 图形变得更容易，它的使用也异常简单，只需要调用 `.animate({})` 这个API即可。 另外一个库 [anime.js](http://anime-js.com/) 通过几行代码就可以让一个元素沿着 SVG 路径运动，点击[这里](https://codepen.io/ainalem/pen/VWJRav) 常看更多 DEMO。

如果你需要一个本身已经为你做了大部分操作来生成复杂的动画的库，[Vivus](https://maxwellito.github.io/vivus/) 是比较适合你的，它采取了一种不同的调用方式，仅需要通过配置项的方式去生成 SVG 路径动画。你只需要用你想作用的 SVG 元素的 id 来新建一个 Vivus 对象，然后就交给 Vivus 来处理剩下的事情。

### 拓展阅读

- 更深入了解 SVG 动画可以阅读这篇文章：[three ways to animate SVG](https://css-tricks.com/video-screencasts/135-three-ways-animate-svg/) ；

- 本文中没有涉及的关于 SMIL（Synchronized Multimedia Integration Language）的内容可以阅读：[A Guide to SVG Animations (SMIL)](https://css-tricks.com/guide-svg-animations-smil/);