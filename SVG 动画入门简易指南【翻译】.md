# SVG 动画入门简易指南

> 原文链接：[https://www.toptal.com/front-end/svg-animation-guide](https://www.toptal.com/front-end/svg-animation-guide)

任何称职的前端工程师都会考虑到不成体系的设备生态所带来的挑战。设备间不同的屏幕尺寸、分辨率和比例使产品难以提供一致的体验。尤其是对于那些对产品有着像素级完美追求的人！
SVG（可缩放的矢量图形）完美地解决了上文中提到的部分问题。尽管 SVG 有它的局限性，但是在某些场景下是非常有用的，如果你有一个好的设计团队，你也可以基于SVG创建一些震撼的视觉体验，而不必担心给浏览器带来过重的渲染负担或阻碍页面的加载时间。

![](http://7xr868.com1.z0.glb.clouddn.com/kjl-toptal-blog-image-1501855758851-cead6880a7875388174043632a50fe07.jpg)

在过去的几个月里，我一直在做一个项目，它广泛使用了 SVG 及其动画和效果。在本文中，我将介绍如何使用SVG及其动画技术为你的Web前端工作带来一些新鲜的体验。

### 可缩放矢量图形
SVG 是一种基于 XML 文档的图片格式，所以大部分情况其表现特性类似于 HTML。它为许多常见的几何图形定义了不同的元素，这些几何形状可以组合在标记中生成二维图形。
[SVG 规范](https://www.w3.org/TR/SVG/)是由万维网联盟（W3C）于1999制定的标准。
所有的主流浏览器目前均支持了 [SVG 的渲染](https://caniuse.com/#search=svg)。
由于 SVG 图形是 XML 文档，因此Web浏览器提供了基于DOM节点的API，同样可用于与 SVG 图形的交互。
### SVG Paths
如果要说出 SVG 中最强大的元素，那一定是 **<path>**  （路径元素） 了。
路径元素是一个基本形状，它可以构建出你所能想象的几乎任何高级的2D图形。路径元素通过一系列绘图命令来生效，它非常类似于1967年的 Logo 编程语言，它只是更为现代化的，为复杂花哨的图形而设计的。这些命令如下图所示被写在路径元素的  **d** 属性中 ：

```html
<!-- A right-angled triangle -->
<path d="M10 10 L75 10 L75 75 Z" />
```

你可以把它想象成有一只虚拟的画笔在屏幕上作画，而路径元素的 d 属性中的绘图命令控制着画笔的走向。上图的示例中，画笔一开始移动到起点坐标 `(10, 10)` （M10 10），以 `(75, 10)` 为终点画直线（L75 10），接着又画一条直线至 `(75, 75)` （L75 75），最后的 `Z` 表示画笔回到起点坐标以闭合路径。

使用一些其他的绘图命令，例如绘圆弧（**A**）、二次贝塞尔曲线（**Q**）、三次贝塞尔曲线（**C**）等等，你可以在 SVG 中创建出很多组合的形状和矢量图形。

你可以点击这里了解更多关于路径元素的知识 >> [path element](http://tutorials.jenkov.com/svg/path-element.html)。

### SVG Paths 与 CSS

也许你会问：“好吧我知道 Paths 很很强大，但是我怎样才能对它做动画呢？”。

因此我们第一步需要利用到 SVG 的两个属性：`stroke-dasharray` 和 `stroke-dashoffset` 。

![](http://7xr868.com1.z0.glb.clouddn.com/kjl-toptal-blog-image-1501855771209-e7241a8962fee8e1e344faa248cade72.jpg)

[stroke-dasharray](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/stroke-dasharray) 属性

