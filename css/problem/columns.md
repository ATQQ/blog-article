# 2. columns 设置列宽和列数

## column-count 内容分列
>column-count: number|auto
* number:列的个数
* auto:根据column-with自动适配

html
```html
<div class="newspaper">
“当我年轻的时候，我梦想改变这个世界；当我成熟以后，我发现我不能够改变这个世界，我将目光缩短了些.
</div>
```

css
```css
.newspaper
{
    /* 分成3列 */
	column-count:3;
}
```

## column-fill 列填充方式
>column-fill: balance|auto;
* balance:列长短平衡
* auto:列顺序填充,他们长度可能不同

html
```html
<div class="demo">
ABCDDD...省略一大段文字
</div>
```

css
```css
.demo{
    column-fill:balance;
}
```

## column-gap 列之间的差距
>column-gap: length|normal
* length:指定的长度
* normal:1em
  
html 
```html
<div class="demo">
ABCDDD...省略一大段文字
</div>
```

css
```css
.demo{
    /* 分成4列 */
    column-count:4;
    /* 列之间的间隔2em */
    column-gap:2em;
}
```

## column-rule 指定列之间的规则：宽度，样式和颜色
>column-rule: column-rule-width column-rule-style column-rule-color
* -width:间隔宽度
* [-style](https://www.runoob.com/cssref/css3-pr-column-rule-style.html):间隔样式
* -color:间隔的颜色

html 
```html
<div class="demo">
ABCDDD...省略一大段文字
</div>
```

css
```css
.demo{
    /* 分成4列 */
    column-count:4;
    /* 列之间的间隔1em */
    column-gap:1em;
    /* 间隔线4px宽,点状.红色 */
    column-rule:4px dotted red;
}
```
### column-rule-color　列之间间隔线颜色

### [column-rule-style](https://www.runoob.com/cssref/css3-pr-column-rule-style.html)  列之间间隔线样式

### column-rule-width  列之间间隔线宽度
>值:thin|medium|thick|length;
* thin:细线
* medium:中等
* thick:粗
* length:指定宽度

## column-span 指定元素跨级列
> column-span:1|all

* 1:跨越1列
* all:跨越所有列

html 
```html
<div class="demo">
    <h1>标题标题标题标题标题标题标题标题...</h1>  
ABCDDD...省略一大段文字
</div>
```

css
```css
.demo h1{
    /* 跨越所有列 */
    column-span:all;
}
.demo{
    /* 分成4列 */
    column-count:4;
    /* 列之间的间隔1em */
    column-gap:1em;
    /* 间隔线4px宽,点状.红色 */
    column-rule:4px dotted red;
}
```

## column-width 列宽度

html 
```html
<div class="demo">
ABCDDD...省略一大sssssssss段文字
</div>
```

css
```css
.demo{
    /* 每一列宽度100px */
    column-width:100px;
}
```

## columns 指定列的宽度和数量
> columns: column-width column-count;

html 
```html
<div class="demo">
ABCDDD...省略一大sssssssss段文字
</div>
```

css
```css
/* 设置列宽度100px,列数量3 */
.demo{
    columns:100px 3;
}

/* 等价于 */
.demo{
    column-width:100px;
    column-count:3;
}
```