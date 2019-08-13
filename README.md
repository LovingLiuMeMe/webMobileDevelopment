##移动端开发的基础知识

### 一.分辨率
像素即一个小方块，它具有特定的位置和颜色。  
图片、电子屏幕（手机、电脑）就是由无数个具有特定颜色和特定位置的小方块拼接而成。  
像素可以作为图片或电子屏幕的最小组成单位。
通常我们所说的分辨率有两种，屏幕分辨率和图像分辨率。
#### 1.1屏幕分辨率
屏幕分辨率指一个屏幕具体由多少个像素点组成。
iPhone XS Max 和 iPhone SE的分辨率分别为`2688 x 1242`和`1136 x 640`。这表示手机分别在垂直和水平上所具有的像素点数。
#### 1.2图像分辨率
我们通常说的图片分辨率其实是指图片含有的像素数，比如一张图片的分辨率为`800 x 400`。这表示图片分别在垂直和水平上所具有的像素点数为`800`和`400`。
同一尺寸的图片，分辨率越高，图片越清晰。
#### 1.3PPI
PPI(Pixel Per Inch)：每英寸包括的像素数。
PPI可以用于描述屏幕的清晰度以及一张图片的质量。
使用PPI描述图片时，PPI越高，图片质量越高，使用PPI描述屏幕时，PPI越高，屏幕越清晰。
在上面描述手机分辨率的图片中，我们可以看到：iPhone XS Max 和 iPhone SE的PPI分别为458和326，这足以证明前者的屏幕更清晰。
由于手机尺寸为手机对角线的长度，我们通常使用如下的方法计算PPI:
PPI=Math.sqrt((Math.pow(1792)+Math.pow(828))/6.06) = 326ppi
iPhone 6的PPI为 ，那它每英寸约含有326个物理像素点。
#### 1.4DPI
DPI(Dot Per Inch)：即每英寸包括的点数。
这里的点是一个抽象的单位，它可以是屏幕像素点、图片像素点也可以是打印机的墨点。
平时你可能会看到使用DPI来描述图片和屏幕，这时的DPI应该和PPI是等价的，DPI最常用的是用于描述打印机，表示打印机每英寸可以打印的点数。
一张图片在屏幕上显示时，它的像素点数是规则排列的，每个像素点都有特定的位置和颜色。
当使用打印机进行打印时，打印机可能不会规则的将这些点打印出来，而是使用一个个打印点来呈现这张图像，这些打印点之间会有一定的空隙，这就是DPI所描述的：打印点的密度。

在上面的图像中我们可以清晰的看到，打印机是如何使用墨点来打印一张图像。
所以，打印机的DPI越高，打印图像的精细程度就越高，同时这也会消耗更多的墨点和时间。
### 二.设备独立像素
实际上，上面我们描述的像素都是物理像素，即设备上真实的物理单元。设置独立像素是一个逻辑值。  
下面我们来看看设备独立像素究竟是如何产生的：
智能手机发展非常之快，在几年之前，我们还用着分辨率非常低的手机，比如下面左侧的白色手机，它的分辨率是320x480，我们可以在上面浏览正常的文字、图片等等。  
但是，随着科技的发展，低分辨率的手机已经不能满足我们的需求了。很快，更高分辨率的屏幕诞生了，比如下面的黑色手机，它的分辨率是640x940，正好是白色手机的两倍。  
理论上来讲，在白色手机上相同大小的图片和文字，在黑色手机上会被缩放一倍，因为它的分辨率提高了一倍。这样，岂不是后面出现更高分辨率的手机，页面元素会变得越来越小吗？  
然而，事实并不是这样的，我们现在使用的智能手机，不管分辨率多高，他们所展示的界面比例都是基本类似的。乔布斯在iPhone4的发布会上首次提出了Retina Display(视网膜屏幕)的概念，它正是解决了上面的问题，这也使它成为一款跨时代的手机。  
在iPhone4使用的视网膜屏幕中，把2x2个像素当1个像素使用，这样让屏幕看起来更精致，但是元素的大小却不会改变。  

如果黑色手机使用了视网膜屏幕的技术，那么显示结果应该是下面的情况，比如列表的宽度为300个像素，那么在一条水平线上，标清手机会用300个物理像素去渲染它，而高清手机实际上会用600个物理像素去渲染它。  
我们必须用一种单位来同时告诉不同分辨率的手机，它们在界面上显示元素的大小是多少，这个单位就是设备独立像素(Device Independent Pixels)简称DIP或DP。上面我们说，列表的宽度为300个像素，实际上我们可以说：列表的宽度为300个设备独立像素。  
#### 2.1 设备像素比DPR
设备像素比device pixel ratio简称dpr，即`物理像素`和`设备独立像素`的比值。
在web中，浏览器为我们提供了window.devicePixelRatio来帮助我们获取dpr。
在css中，可以使用媒体查询`min-device-pixel-ratio`，区分dpr：
```css
@media (-webkit-min-device-pixel-ratio: 2),(min-device-pixel-ratio: 2){ }
```
复制代码在`React Native`中，我们也可以使用`PixelRatio.get()`来获取`DPR`。
当然，上面的规则也有例外，iPhone 6、7、8 Plus的实际物理像素是`1080 x 1920`，在开发者工具中我们可以看到：它的设备独立像素是`414 x 736`，设备像素比为`3`，设备独立像素和设备像素比的乘积并不等于`1080 x 1920`，而是等于`1242 x 2208`。
实际上，手机会自动把`1242 x 2208`个像素点塞进`1080 * 1920`个物理像素点来渲染，我们不用关心这个过程，而`1242 x 2208`被称为屏幕的设计像素。我们开发过程中也是以这个设计像素为准。
实际上，从苹果提出视网膜屏幕开始，才出现设备像素比这个概念，因为在这之前，移动设备都是直接使用物理像素来进行展示。
紧接着，Android同样使用了其他的技术方案来实现DPR大于1的屏幕，不过原理是类似的。由于Android屏幕尺寸非常多、分辨率高低跨度非常大，不像苹果只有它自己的几款固定设备、尺寸。所以，为了保证各种设备的显示效果，Android按照设备的像素密度将设备分成了几个区间：

当然，所有的Android设备不一定严格按照上面的分辨率，每个类型可能对应几种不同分辨率，所以，每个Android手机都能根据给定的区间范围，确定自己的DPR，从而拥有类似的显示。当然，`仅仅是类似`，由于各个设备的尺寸、分辨率上的差异，设备独立像素也不会完全相等，所以各种Android设备仍然不能做到在展示上完全相等。
#### 2.2移动端开发
在iOS、Android和React Native开发中样式单位其实都使用的是设备独立像素。
`iOS`的尺寸单位为`pt`，`Android`的尺寸单位为`dp`，`React Native`中没有指定明确的单位，它们其实都是设备独立像素`dp`。
在使用React Native开发App时，UI给我们的原型图一般是基于iphone6的像素给定的。
为了适配所有机型，我们在写样式时需要把`物理像素`转换为`设备独立像素`：例如：如果给定一个元素的高度为200px(这里的px指物理像素，非CSS像素)，iphone6的设备像素比为2，我们给定的height应为200px/2=100dp。
当然，最好的是，你可以和设计沟通好，所有的UI图都按照设备独立像素来出。
我们还可以在代码(React Native)中进行px和dp的转换：
```js
import {PixelRatio } from 'react-native';

const dpr = PixelRatio.get();

/**
 * px转换为dp
 */
export function pxConvertTodp(px) {
  return px / dpr;
}

/**
 * dp转换为px
 */
export function dpConvertTopx(dp) {
  return PixelRatio.getPixelSizeForLayoutSize(dp);
}
```
#### 2.3WEB端开发
在写CSS时，我们用到最多的单位是px，即CSS像素，当页面缩放比例为100%时，一个CSS像素等于一个设备独立像素。
但是CSS像素是很容易被改变的，当用户对浏览器进行了放大，CSS像素会被放大，这时一个CSS像素会跨越更多的物理像素。
页面的缩放系数 = CSS像素 / 设备独立像素。
### 三.视口
视口(viewport)代表当前可见的计算机图形区域。在Web浏览器术语中，通常与浏览器窗口相同，但不包括浏览器的UI， 菜单栏等——即指你正在浏览的文档的那一部分。
一般我们所说的视口共包括三种：`布局视口`、`视觉视口`和`理想视口`，它们在屏幕适配中起着非常重要的作用。

#### 3.1 布局视口
布局视口(layout viewport)：当我们以百分比来指定一个元素的大小时，它的计算值是由这个元素的包含块计算而来的。当这个元素是最顶级的元素时，它就是基于布局视口来计算的。
所以，布局视口是网页布局的基准窗口，在PC浏览器上，布局视口就等于当前浏览器的窗口大小（不包括borders 、margins、滚动条）。
在移动端，布局视口被赋予一个默认值，大部分为980px，这保证PC的网页可以在手机浏览器上呈现，但是非常小，用户可以手动对网页进行放大。
我们可以通过调用`document.documentElement.clientWidth` / `clientHeight`来获取布局视口大小。

#### 3.2 视觉视口
视觉视口(visual viewport)：用户通过屏幕真实看到的区域。
视觉视口默认等于当前浏览器的窗口大小（包括滚动条宽度）。
当用户对浏览器进行缩放时，不会改变`布局视口`的大小，所以页面布局是不变的，但是缩放会改变`视觉视口`的大小。
例如：用户将浏览器窗口放大了200%，这时浏览器窗口中的`CSS像素`会随着`视觉视口`的放大而放大，这时一个`CSS像素`会跨越更多的`物理像素`。
所以，**布局视口会限制你的CSS布局而视觉视口决定用户具体能看到什么**。
我们可以通过调用`window.innerWidth` / `innerHeight`来获取视觉视口大小。

#### 3.3 理想视口
布局视口在移动端展示的效果并不是一个理想的效果，所以理想视口(ideal viewport)就诞生了：网站页面在移动端展示的理想大小。
如上图，我们在描述设备独立像素时曾使用过这张图，在浏览器调试移动端时页面上给定的像素大小就是理想视口大小，它的单位正是设备独立像素。
上面在介绍CSS像素时曾经提到页面的缩放系数 = CSS像素 / 设备独立像素，实际上说页面的缩放系数 = 理想视口宽度 / 视觉视口宽度更为准确。
所以，当页面缩放比例为100%时，CSS像素 = 设备独立像素，理想视口 = 视觉视口。
我们可以通过调用screen.width / height来获取理想视口大小。

#### 3.4 Meta viewport
`<meta>` 元素表示那些不能由其它HTML元相关元素之一表示的任何元数据信息，它可以告诉浏览器如何解析页面。
我们可以借助`<meta>`元素的`viewport`来帮助我们`设置视口`、`缩放`等，从而让移动端得到更好的展示效果。
```html
<meta name="viewport" content="width=device-width; initial-scale=1; maximum-scale=1; minimum-scale=1; user-scalable=no;">
```
复制代码上面是viewport的一个配置，我们来看看它们的具体含义:  
| Value  | 可能值 | 描述 | 
| - | :-: | -: | 
| width | 正整数或device-width| 以pixels（像素）为单位， 定义布局视口的宽度。 | 
| height  | 正整数或device-height | 以pixels（像素）为单位， 定义布局视口的高度。 | 
| initial-scale | 0.0 - 10.0 | 定义页面初始缩放比率。 |
| minimum-scale | 0.0 - 10.0 | 定义缩放的最小值；必须小于或等于maximum-scale的值。 |
| maximum-scale| 0.0 - 10.0 | 定义缩放的最大值；必须大于或等于minimum-scale的值。 |
| user-scalable | 一个布尔值（yes或者no） | 如果设置为 no，用户将不能放大或缩小网页。默认值为 yes。 |

#### 3.5移动端适配
为了在移动端让页面获得更好的显示效果，我们必须让布局视口、视觉视口都尽可能等于理想视口。
`device-width`就等于理想视口的宽度，所以设置`width=device-width`就相当于让布局视口等于理想视口。
由于`initial-scale = 理想视口宽度 / 视觉视口宽度`，所以我们设置`initial-scale=1`;就相当于让视觉视口等于理想视口。
这时，1个CSS像素就等于1个设备独立像素，而且我们也是基于理想视口来进行布局的，所以呈现出来的页面布局在各种设备上都能大致相似。

#### 3.6缩放
上面提到width可以决定布局视口的宽度，实际上它并不是布局视口的唯一决定性因素，设置`initial-scale`也有肯能影响到布局视口，因为布局视口宽度取的是width和视觉视口宽度的最大值。
例如：若手机的理想视口宽度为`400px`，设置`width=device-width`，`initial-scale=2`，此时`视觉视口宽度 = 理想视口宽度 / initial-scale`即200px，布局视口取两者最大值即`device-width 400px`。
若设置`width=device-width`，`initial-scale=0.5`，此时`视觉视口宽度 = 理想视口宽度 / initial-scale`即800px，布局视口取两者最大值即800px。
#### 3.7获取浏览器大小
浏览器为我们提供的获取窗口大小的API有很多，下面我们再来对比一下：
```css
window.innerHeight：获取浏览器视觉视口高度（包括垂直滚动条）。
window.outerHeight：获取浏览器窗口外部的高度。表示整个浏览器窗口的高度，包括侧边栏、窗口镶边和调正窗口大小的边框。
window.screen.Height：获取获屏幕取理想视口高度，这个数值是固定的，设备的分辨率/设备像素比
window.screen.availHeight：浏览器窗口可用的高度。
document.documentElement.clientHeight：获取浏览器布局视口高度，包括内边距，但不包括垂直滚动条、边框和外边距。
document.documentElement.offsetHeight：包括内边距、滚动条、边框和外边距。
document.documentElement.scrollHeight：在不使用滚动条的情况下适合视口中的所有内容所需的最小宽度。测量方式与clientHeight相同：它包含元素的内边距，但不包括边框，外边距或垂直滚动条。
```
### 四.1px问题
为了适配各种屏幕，我们写代码时一般使用设备独立像素来对页面进行布局。

而在设备像素比大于1的屏幕上，我们写的1px实际上是被多个物理像素渲染，这就会出现1px在有些屏幕上看起来很粗的现象。

#### 4.1 border-image
基于media查询判断不同的设备像素比给定不同的border-image：
```css
       .border_1px{
          border-bottom: 1px solid #000;
        }
        @media only screen and (-webkit-min-device-pixel-ratio:2){
            .border_1px{
                border-bottom: none;
                border-width: 0 0 1px 0;
                border-image: url(../img/1pxline.png) 0 0 2 0 stretch;
            }
        }
```
#### 4.2 background-image
和border-image类似，准备一张符合条件的边框背景图，模拟在背景上。
```css
       .border_1px{
          border-bottom: 1px solid #000;
        }
        @media only screen and (-webkit-min-device-pixel-ratio:2){
            .border_1px{
                background: url(../img/1pxline.png) repeat-x left bottom;
                background-size: 100% 1px;
            }
        }
```
上面两种都需要单独准备图片，而且圆角不是很好处理，但是可以应对大部分场景。
#### 4.3 伪类 + transform
基于media查询判断不同的设备像素比对线条进行缩放：
```css
       .border_1px:before{
          content: '';
          position: absolute;
          top: 0;
          height: 1px;
          width: 100%;
          background-color: #000;
          transform-origin: 50% 0%;
        }
        @media only screen and (-webkit-min-device-pixel-ratio:2){
            .border_1px:before{
                transform: scaleY(0.5);
            }
        }
        @media only screen and (-webkit-min-device-pixel-ratio:3){
            .border_1px:before{
                transform: scaleY(0.33);
            }
        }
```
这种方式可以满足各种场景，如果需要满足圆角，只需要给伪类也加上`border-radius`即可。
#### 4.4 svg
上面我们`border-image`和`background-image`都可以模拟1px边框，但是使用的都是位图，还需要外部引入。
借助`PostCSS`的`postcss-write-svg`我们能直接使用`border-image`和`background-image`创建`svg`的`1px`边框：
```css
@svg border_1px { 
  height: 2px; 
  @rect { 
    fill: var(--color, black); 
    width: 100%; 
    height: 50%; 
    } 
  } 
.example { border: 1px solid transparent; border-image: svg(border_1px param(--color #00b1ff)) 2 2 stretch; }
```
编译后：
```css
.example { border: 1px solid transparent; border-image: url("data:image/svg+xml;charset=utf-8,%3Csvg xmlns='http://www.w3.org/2000/svg' height='2px'%3E%3Crect fill='%2300b1ff' width='100%25' height='50%25'/%3E%3C/svg%3E") 2 2 stretch; }
```
复制代码上面的方案是大漠在他的文章中推荐使用的，基本可以满足所有场景，而且不需要外部引入，这是我个人比较喜欢的一种方案。
#### 4.5 设置viewport
通过设置缩放，让CSS像素等于真正的物理像素。
例如：当设备像素比为3时，我们将页面缩放1/3倍，这时1px等于一个真正的屏幕像素。
```js
    const scale = 1 / window.devicePixelRatio;
    const viewport = document.querySelector('meta[name="viewport"]');
    if (!viewport) {
        viewport = document.createElement('meta');
        viewport.setAttribute('name', 'viewport');
        window.document.head.appendChild(viewport);
    }
    viewport.setAttribute('content', 'width=device-width,user-scalable=no,initial-scale=' + scale + ',maximum-scale=' + scale + ',minimum-scale=' + scale);
```
实际上，上面这种方案是早先flexible采用的方案。
当然，这样做是要付出代价的，这意味着你页面上所有的布局都要按照物理像素来写。这显然是不现实的，这时，我们可以借助`flexible`或`vw`、`vh`来帮助我们进行适配。
### 五.移动端适配方案
尽管我们可以使用设备独立像素来保证各个设备在不同手机上显示的效果类似，但这并不能保证它们显示完全一致，我们需要一种方案来让设计稿得到更完美的适配。
#### 5.1 flexible方案
flexible方案是阿里早期开源的一个移动端适配解决方案，引用flexible后，我们在页面上统一使用rem来布局。
它的核心代码非常简单：
```js
// set 1rem = viewWidth / 10
function setRemUnit () {
    var rem = docEl.clientWidth / 10
    docEl.style.fontSize = rem + 'px'
}
setRemUnit();
```
rem 是相对于html节点的`font-size`来做计算的。
我们通过设置`document.documentElement.style.fontSize`就可以统一整个页面的布局标准。
上面的代码中，将html节点的`font-size`设置为页面`clientWidth(布局视口)`的`1/10`，即`1rem`就等于页面布局视口的`1/10`，这就意味着我们后面使用的`rem`都是按照页面比例来计算的。
这时，我们只需要将UI出的图转换为`rem`即可。
以iPhone6为例：布局视口为`375px`，则`1rem = 37.5px`，这时`UI`给定一个元素的宽为`75px`（设备独立像素），我们只需要将它设置为`75 / 37.5 = 2rem`。
当然，每个布局都要计算非常繁琐，我们可以借助`PostCSS`的`px2rem`插件来帮助我们完成这个过程。
下面的代码可以保证在页面大小变化时，布局可以自适应，当触发了window的resize和pageShow事件之后自动调整html的fontSize大小。
```js
  // reset rem unit on page resize
window.addEventListener('resize', setRemUnit)window.addEventListener('pageshow', function (e) {
    if (e.persisted) {
      setRemUnit()
    }
})
```
由于viewport单位得到众多浏览器的兼容，上面这种方案现在已经被官方弃用：

lib-flexible这个过渡方案已经可以放弃使用，不管是现在的版本还是以前的版本，都存有一定的问题。建议大家开始使用viewport来替代此方案。

下面我们来看看现在最流行的vh、vw方案。
#### 5.2 vh、vw方案
vh、vw方案即将`视觉视口宽度 window.innerWidth`和`视觉视口高度 window.innerHeight`等分为 `100` 份。
上面的`flexible`方案就是模仿这种方案，因为早些时候`vw`还没有得到很好的兼容。
```css
vw(Viewport's width)：1vw等于视觉视口的1%
vh(Viewport's height) :1vh 为视觉视口高度的1%
vmin :  vw 和 vh 中的较小值
vmax : 选取 vw 和 vh 中的较大值
```
如果视觉视口为`375px`，那么1vw = 3.75px，这时UI给定一个元素的宽为`75px`（设备独立像素），我们只需要将它设置为`75 / 3.75 = 20vw`。
这里的比例关系我们也不用自己换算，我们可以使用`PostCSS`的 `postcss-px-to-viewport` 插件帮我们完成这个过程。写代码时，我们只需要根据UI给的设计图写`px`单位即可。
当然，没有一种方案是十全十美的，vw同样有一定的缺陷：
```
px转换成vw不一定能完全整除，因此有一定的像素差。
比如当容器使用vw，margin采用px时，很容易造成整体宽度超过100vw，从而影响布局效果。当然我们也是可以避免的，例如使用padding代替margin，结合calc()函数使用等等...
```
### 六.适配iPhoneX
iPhoneX的出现将手机的颜值带上了一个新的高度，它取消了物理按键，改成了底部的小黑条，但是这样的改动给开发者适配移动端又增加了难度。
#### 6.1 安全区域
在iPhoneX发布后，许多厂商相继推出了具有边缘屏幕的手机。

这些手机和普通手机在外观上无外乎做了三个改动：`圆角（corners）`、`刘海（sensor housing`）和`小黑条（Home Indicator）`。为了适配这些手机，安全区域这个概念变诞生了：`安全区域`就是一个不受上面三个效果的`可视窗口范围`。
为了保证页面的显示效果，我们必须把页面限制在安全范围内，但是不影响整体效果。
#### 6.2 viewport-fit
`viewport-fit`是专门为了适配`iPhoneX`而诞生的一个属性，它用于限制网页如何在安全区域内进行展示。

`contain`: 可视窗口完全包含网页内容
`cover`：网页内容完全覆盖可视窗口
默认情况下或者设置为`auto`和`contain`效果相同。
#### 6.3 env、constant
我们需要将顶部和底部合理的摆放在安全区域内，`iOS11`新增了两个CSS函数`env`、`constant`，用于设定安全区域与边界的距离。
函数内部可以是四个常量：
```css
safe-area-inset-left：安全区域距离左边边界距离
safe-area-inset-right：安全区域距离右边边界距离
safe-area-inset-top：安全区域距离顶部边界距离
safe-area-inset-bottom：安全区域距离底部边界距离
```
注意：我们必须指定viweport-fit后才能使用这两个函数：
```html
<meta name="viewport" content="viewport-fit=cover">
```
`constant`在iOS < 11.2的版本中生效，`env`在iOS >= 11.2的版本中生效，这意味着我们往往要同时设置他们，将页面限制在安全区域内：
```css
body {
  padding-bottom: constant(safe-area-inset-bottom);
  padding-bottom: env(safe-area-inset-bottom);
}
```
复制代码当使用底部固定导航栏时，我们要为他们设置padding值：
```css
{
  padding-bottom: constant(safe-area-inset-bottom);
  padding-bottom: env(safe-area-inset-bottom);
}
```
### 七.横屏适配

很多视口我们要对横屏和竖屏显示不同的布局，所以我们需要检测在不同的场景下给定不同的样式：
#### 7.1 JavaScript检测横屏
window.orientation:获取屏幕旋转方向
```js
window.addEventListener("resize", ()=>{
    if (window.orientation === 180 || window.orientation === 0) { 
      // 正常方向或屏幕旋转180度
        console.log('竖屏');
    };
    if (window.orientation === 90 || window.orientation === -90 ){ 
       // 屏幕顺时钟旋转90度或屏幕逆时针旋转90度
        console.log('横屏');
    }  
}); 
```
#### 7.2 CSS检测横屏
```css
@media screen and (orientation: portrait) {
  /*竖屏...*/
} 
@media screen and (orientation: landscape) {
  /*横屏...*/
}
```

1.物理像素
```
手机的分辨率 1920*1080 
是指屏幕的宽为1920个像素点 高为1080个像素点
```
2.css像素
```
css像素是指的我们开发中所使用的像素
#id{
  width:100px;
  height:100px;
}
```
css像素和物理像素的关系
在标清屏幕下和在高清屏幕下 css大小其实是一样的。只不过在标清屏下 只需要使用两个像素点即可一显示全部的宽或高，但是高清屏下需要使用4个像素点，是的显示的内容的精度更高。
3.设备像素比(dpr)
dpr = 设备像素/css像素(缩放比是1的情况下)
dpr等于2 表示1个css像素用2X2个设备像素来绘制
标清 dpr = 1/1 高清 dpr = 2/1

4.缩放
缩放改变的是css像素的大小.即css像素和物理像素的比例
5.PPI
每英寸的物理像素点（物理像素密度）
ppi=Math.sqrt((Math.pow(1792)+Math.pow(828))/6.06) = 326ppi
屏幕的横向像素 屏幕的竖向像素 屏幕对角线英寸距离
