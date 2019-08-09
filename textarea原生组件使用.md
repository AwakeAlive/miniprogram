# textarea 原生组件层级高
> 场景：textarea原生组件层级比较高，在自定义导航栏下，或者显示弹窗下，textarea文字层级高，会发现透视，文字穿透情况。
解决方案: `textarea` 与 `view` 交替使用
默认`view`，模拟`textarea`，当点击`view`时，`view`隐藏，`textarea`显示，同时聚焦，拉起键盘。

优化点：
* `textarea`空时，`view`模拟`textarea` `placeholder`
* `textarea`显示时，延时300ms，聚焦
```html
<view>
  <view class="my_textarea textarea_view" catchtap="textareaFocus" wx:if="{{!textareaFocus}}">{{textareaValue}}</view>
  <textarea wx:else
    placeholder="请说明"
    focus="{{focus}}"
    maxlength="50"
    placeholder-class="input_placeholder"
    bindblur="textareaInputBlur"
    cursor-spacing="120px"
    value="{{textareaValue}}"
    class="my_textarea"
    fixed="true"
  >
  </textarea>
</view>
```
```css
.my_textarea {
  position: relative;
}
/* 模拟placehoder */
.textarea_view:empty::after {
  positon: absolute;
  content: '请说明';
  left: 24rpx;
  top: 20rpx;
  color: #999;
  font-size: 28rpx;
}
```
```js
// textarea 与view toggleShow
  textareaFocus() {
    this.setData({
      textareaFocus: true,
    }, () => {
      setTimeout(() => {
        this.setData({
          focus: true
        }, 300)
      })
    })
  },
```