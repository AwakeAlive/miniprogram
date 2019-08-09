# modal滑动穿透问题 catchtouchmove
> 场景 当底部page内容多时，出现滚动条，modal层可以考虑加入catchtouchmove 阻止底层内容滑动
遇到问题：当前弹窗内容也有需要滚动条时，由于最外层catchtouchmove同样会阻止子级节点滑动，可以考虑，子级节点采用scroll-view
```html
<view class="mask" catchtouchmove>
  <view class="bd">
    <scroll-view scroll-y class="scroll_view" style="height: {{height}}px"></scroll-view>
  </view>
</view>
```
```css
.scroll_view {
  -webkit-overflow-scrolling: touch; // 开启ios 惯性滚动 流畅性
}
```