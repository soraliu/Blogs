# CSS
## 多列布局
### 3列布局
1. [两端固定，中间自适应](code/css/layout-mid-response.html);
2. [两端自适应，中间固定](code/css/layout-mid-fixed.html);

### position
1. position基本属性：
    * static
    * relative
    * absolute
    * fixed
    * inherit
2. position注意事项[`code`](code/css/position-absolute.html);
  * `absolute`内嵌`absolute`中，内部布局初始定位点是相对于最近`absolute/relative`布局父元素
  * `left``right``top``bottom`对`position:static`无效
  * `position:relative`元素设置`left``right``top``bottom`后仍然占用原始文档位置，而非偏移后的位置。