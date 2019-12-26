# 1. box-shadow 阴影

boxShadow 属性把一个或多个下拉阴影添加到框上

## 格式
```css
box-shadow: h-shadow v-shadow blur spread color inset;
```
* h-shadow(必):水平阴影位置
* v-shadow(必)):垂直阴影位置
* blur(选):模糊距离
* spread(选):阴影大小
* color(选):阴影颜色
* inset(选):内侧的阴影

## 例子
```css
/* 外阴影 */
.test1{
    box-shadow :-5px 0 0 0 deeppink;
}
/* 内阴影 */
.test2{
    box-shadow:inset 5px 0 0 0 deepp
}
```