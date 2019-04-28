### 1.滚动捕捉

## 什么是捕捉？
精确的操控滚动的体验。
```ruby
img {
    /* 指定每张图片的捕捉位置与滚动容器可视区域 x 轴中心的位置对齐 */
    scroll-snap-align: none center;
}
.photoGallery {
    width: 500px;
    overflow-x: auto;
    overflow-y: hidden;
    white-space: nowrap;
    /* 要求每次滚动的结束的位置精确地落在捕捉点上 */
    scroll-snap-type: x mandatory;
}
```
图中的红色区域即为可滚动容器的可视区域或叫捕捉视口。图片黄色框的地方被称为捕捉区域。我们上面设置的 scroll-snap-align 中指定了横轴捕捉点为中心位置。此时将在捕捉视口区域中心（红色虚线）以及捕捉区域中心（黄色虚线）形成捕捉点。


```ruby
.page {
    /* 指定每一页文档的顶部为捕捉的位置 */
    scroll-snap-align: start none;
}
.docScroller {
    width: 500px;
    overflow-x: hidden;
    overflow-y: auto;
    /* 指定捕捉视口应有 100px 的上内边距 */
    scroll-padding: 100px 0 0;
    /* 使用非精确捕捉，能允许滚动最终停止在捕捉点的附近而不需要进一步的调整 */
    scroll-snap-type: y proximity;
}

```
## 总结
scroll-snap-type将一个滚动容器变成滚动捕捉容器，可以设置捕捉程度。
- none不捕捉
- mandatory精确捕捉
- proximity非精确捕捉
捕捉轴方向
- x: 捕捉影响捕捉视口 x 轴方向
- y: 捕捉影响捕捉视口的 y 轴方向
- block: 捕捉影响块轴方向
- inline: 捕捉影响行轴方向
- both: 捕捉分别影响所有方向
```ruby
 scroll-snap-type: y proximity; y轴非精确捕捉
```
scroll-snap-align捕捉位置和对齐位置。
滚动区域和捕捉视口的捕捉位置，并且作为所有捕捉区域相对与捕捉视口的对齐位置。
- none: 未定义
- start: 对齐开始位置
- end: 对齐结束位置
- center: 对齐中心位置
```ruby
 scroll-snap-align: none center;
```