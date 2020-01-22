# 3. 伪元素after与before

## 1.指定文本前后添加内容

![图片](http://img.cdn.sugarat.top/mdImg/MTU3OTYxMzk2Nzc5MQ==579613967791)

```html
<div class="box">test</div>
```

```css
.box::before{
    content: 'before';
    margin-right:10px ;
}
.box::after{
    content: 'after';
    margin-left:10px ;
}
```

## 2. 实现一个书签标记logo

![图片](http://img.cdn.sugarat.top/mdImg/MTU3OTYxNDYyMjQ1MA==579614622450)

```html
<div class="mark">
    标<br>记
</div>
```

```less
.mark{
    width: 30px;
    height: 55px;
    color: #fff;
    border-radius: 3px 3px 0 0;
    background-color: red;
    text-align: center;
    position: relative;
    &::after,&::before{
        position: absolute;
        content: '';
        display: block;
        border: 15px solid transparent;
    }
    &::after{
        right: 0;
        border-right: 15px solid red;
        bottom: -15px;
    }
    &::before{
        left: 0;
        border-left: 15px solid red;
        bottom: -15px;
    }
}
```

## 3.文字前后自动加上引号

![图片](http://img.cdn.sugarat.top/mdImg/MTU3OTY5ODc2MzUyNw==579698763527)
```html
<div class="code">
    content
</div>
```

```less
.code{
    margin-top: 20px;
    &::before{
        content: "\"";
        color: red;
    }
    &::after{
        content: "\"";
        color: blue;
    }
    &:hover{
        &::before,&::after{
            background-color: yellowgreen;
        }
    }
}
```

## 4.自定义样式实现checkbox

![图片](http://img.cdn.sugarat.top/mdImg/MTU3OTY5ODk0MjY3MA==579698942670)

```html
<label for="moreAction">
    <input id="moreAction" type="checkbox">
    <span class="fw-checkbox"></span>
    <span>测试</span>   
</label>
```

```less
#moreAction{
    display: none;
}
.fw-checkbox{
    position: relative;
    display: inline-block;
    box-sizing: border-box;
    width: 15px;
    height: 15px;
    border: 1px solid #000;
    border-radius: 2px;
}

#moreAction[type="checkbox"]:checked{
    +.fw-checkbox{
        background-color: crimson;
    }
    +.fw-checkbox::before{
        position: absolute;
        display: inline-block;
        content: '';
        width: 6px;
        height: 10px;
        border-right: 2px solid #fff;
        border-bottom: 2px solid #fff;
        transform: rotate(45deg);
        left: 3px;
        bottom: 3px;
    }
}
```

5. 简单实现一个聊天气泡

![图片](http://img.cdn.sugarat.top/mdImg/MTU3OTY5OTE2Nzc5OQ==579699167799)

```html
<div class="bubble">
    <img src="http://img.cdn.sugarat.top/mdImg/MTU3OTY5OTUwMDM1Mw==579699500353" alt="">
    <div class="chat">66666!!!</div>
</div>
```

```less
.bubble{
    display: flex;
    align-items: center;
    img{
        width: 40px;
        height: 40px;
        border-radius: 50%;
        margin-right: 20px;
    }
    .chat{
        position: relative;
        background-color: cyan;
        padding: 6px;
        border-radius: 4px;
        box-sizing: border-box;
        &::before{
            content: '';
            position: absolute;
            left: -20px;
            border: 10px solid transparent;
            border-right: 10px solid cyan;
        }
    }
}
```

## 6. 相片堆叠

![图片](http://img.cdn.sugarat.top/mdImg/MTU3OTY5OTQwOTYxMQ==579699409611)

```html
 <div class="img-area">
    <div class="pic">
            <img src="http://img.cdn.sugarat.top/mdImg/MTU3OTY5OTUwMDM1Mw==579699500353" alt="">
        </div>
</div>
```

```less
.pic{
    position: relative;
    img{
        width: 100%;
        height: 100%;
    }
}

.pic,
.pic::after,
.pic::before{
    width: 200px;
    height: 150px;
    border: 6px solid #fff;
    box-shadow: 2px 2px 5px #ddd;
}

.pic::after,
.pic::before{
    content: '';
    position: absolute;
    background:#eff4de ;
    border: 6px solid #fff;
}

.pic::after{
    transform: rotate(-2deg);
    z-index: -2;
    left: 0px;
}

.pic::before{
    transform: rotate(-5deg);
    z-index: -1;
    left: 0px;
}
```