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
### 原生组件使用注意点
* 原生组件层级最高，不受`z-index`影响，后插入的原生组件可以覆盖之前的原生组件。（`cover-view`和`cover-image` 可以覆盖部分原生组件上面）
* 原生组件无法在`picker-view`使用（基础库版本2.4.4以下，同样不支持在`scroll-view`, `swiper`, `movable-view`使用）
* 原生组件某些css不支持（1. CSS动画 2. 无法设置原生组件`position: fixed;` 3. 父级节点不能使用`overflow: hidden`来裁剪原生组件显示区域）
* 原生组件事件监听不支持`bind:eventname`，只支持`bindeventname`。同时也不支持`catch`和`capture`的事件绑定方式
* 原生组件有： 
  * camera
  * canvas
  * input(仅focus表现为原生组件)
  * live-player
  * live-pusher
  * map
  * textarea
  * video