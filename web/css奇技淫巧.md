## [CSS奇技淫巧](https://github.com/jawil/blog/issues/29)
- 利用 CSS 的 content 属性 attr 抓取资料 \
```
<div data-msg="Open this file in Github Desktop"> hover </div
div:hover:after{
    content:attr(data-msg);
}
```
- 利用 :valid 和 :invalid 来做表单即时校验
```
input, textarea {
    &:valid {}
    &:invalid {} 
}
```
- 文字像古诗一样竖着呈现，`writing-mode: vertical-rl`
- 利用 pointer-events 禁用 a 标签事件效果
- CSS 如何实现文字两端对齐，`text-align-last: justify`
- 利用 transparent 属性实现各种三角形，提示框
```
#triangle-right {
    width: 0;
    height: 0;
    border-top: 50px solid transparent;
    border-left: 100px solid red;
    border-bottom: 50px solid transparent;
}
```
- 所有图片变成黑白色彩，`filter: grayscale(100%)`
- 



