---
title: background-origin
slug: Web/CSS/background-origin
---

{{CSSRef}}

`background-origin` 规定了指定背景图片{{cssxref("background-image")}} 属性的原点位置的背景相对区域。

{{EmbedInteractiveExample("pages/css/background-origin.html")}}

注意：当使用 {{Cssxref("background-attachment")}} 为 fixed 时，该属性将被忽略不起作用。

## 语法

```css
/* 关键值 */
background-origin: border-box;
background-origin: padding-box;
background-origin: content-box;

/* 全局值 */
background-origin: inherit;
background-origin: initial;
background-origin: revert;
background-origin: revert-layer;
background-origin: unset;
```

`background-origin` 属性被指定为下面列出的关键字值之一。

### 属性值

- `border-box`
  - : 背景图片的摆放以 border 区域为参考
- `padding-box`
  - : 背景图片的摆放以 padding 区域为参考
- `content-box`
  - : 背景图片的摆放以 content 区域为参考

## 正式定义

{{cssinfo}}

## 正式语法 

{{csssyntax}}

## 例子

### 设置 background origins

```css
.example {
  border: 10px double;
  padding: 10px;
  background: url("image.jpg");
  background-position: center left;
   /* 背景将在内容区 padding 内部填充 */
  background-origin: content-box;
}
```

```css
#example2 {
  border: 4px solid black;
  padding: 10px;
  background: url("image.gif");
  background-repeat: no-repeat;
  background-origin: border-box;
}
```

```css
div {
  background-image: url("logo.jpg"), url("mainback.png"); /* 将两张图片应用于背景 */
  background-position: top right, 0px 0px;
  background-origin: content-box, padding-box;
}
```

### 使用两个渐变色

在这个例子中，box 有一个粗虚线边框。第一个渐变色使用“padding-box”作为“background-origin”，因此背景位于边框内。第二个使用“content-box”，因此只显示在内容后面。

```css
.box {
  margin: 10px 0;
  color: #fff;
  background: linear-gradient(
      90deg,
      rgba(131, 58, 180, 1) 0%,
      rgba(253, 29, 29, 0.6) 60%,
      rgba(252, 176, 69, 1) 100%
    ), radial-gradient(circle, rgba(255, 255, 255, 1) 0%, rgba(0, 0, 0, 1) 28%);
  border: 20px dashed black;
  padding: 20px;
  width: 400px;
  background-origin: padding-box, content-box;
  background-repeat: no-repeat;
}
```

```html
<div class="box">Hello!</div>
```

{{EmbedLiveSample('Using_two_gradients', 420, 140)}}

## 规范

{{Specifications}}

## 浏览器兼容性

{{Compat}}

## 更多

- {{cssxref("background-clip")}}

