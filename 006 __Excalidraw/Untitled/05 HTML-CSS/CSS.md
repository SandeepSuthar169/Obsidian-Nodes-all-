### ~={purple}1. What are the different types of CSS?=~
- *Inline CSS* -> CSS directly inside the HTML element using the `style` attribute.
```HTML
<p style="color: blue; font-size: 18px;">
  This is inline CSS
</p>
```

- Internal CSS -> CSS written inside the `<style>` tag in the `<head>` section of the HTML .
```HTML
<!DOCTYPE html>
<html>
<head>
  <style>
    p {
      color: green;
      font-size: 18px;
    }
  </style>
</head>
<body>

<p>This is internal CSS</p>

</body>
</html>
```

- External CSS -> CSS is written in separate `.css` file 
---
### ~={purple}2. What is the CSS Box Model?=~
- CSS Box Model is a fundamental concept that describes how every HTML element is structured and how space is calculated around it.
#### 1. Content 
- The actual text or image inside the element 
#### 2. Padding
- Space between content and border
```CSS
padding: 10px
```

#### 3. Border
- Surrounds the padding and content
```CSS
border: 2px solid black;
```

#### 4. Margin
- Space outside the border
```CSS
margin: 20px;
```

---
### ~={purple}3. What is specificity in CSS?=~
    Priority order:  
    Inline > ID > Class > Element

---
### ~={purple}4. What is `display` property?=~
- What is `display`?
	- Whether an element takes full width or not and appears on a new line 
#### 1. `block` -> takes full width and starts on a new line 
```CSS
div {  
	display: block;  
}
👉 Examples: <div>, <p>, <h1>
```

#### 2. `inline` -> Takes only required width and does not start on a new line 
```CSS
span {  
	display: inline;  
}
Examples: <span>, <a>
```

#### 3. `inline-block` -> same line and but allows width & height
```CSS
button {  
	display: inline-block;  
}
```

#### 4. `none` -> Hides the element completely and no space is taken 
```CSS
div {  
	display: none;  
}
```

---
### ~={purple}5. Difference between `class` and `id`?=~
- `id` is Unique
- `Class` can be used multiple times
---
### ~={purple}6. What is Flexbox?=~
- is a one dimensional layout system used to align an distribute space among items in a container efficiently. 
```HTML
<div class="container">
  <div>Item 1</div>
  <div>Item 2</div>
  <div>Item 3</div>
</div>
```
```CSS
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 10px;
}
```

---
### ~={purple}7. What is Grid?=~
- is a two dimensional layouts user with rows and columns at the same time. 
```HTML
<div class="container">  
	<div>1</div>  
	<div>2</div>  
	<div>3</div>  
</div>
```
```CSS
.container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 10px;
}
```

---
### ~={purple}8. What is `position` property?=~
- is used to control how an element is position on a webpage.
- Types of Position 
1. `Static` -> elements follow normal flow
```CSS
div {
  position: static;
}
```

2. `relative` -> Position relative to its original position 
```CSS
div {
  position: relative;
  top: 10px;
  left: 20px;
}
```

3. `absolute` -> Position relative to the nearest position parent
```CSS
div {
  position: absolute;
  top: 0;
  left: 0;
}
```

4. `fixex` -> position relative to the screen stays in place when scrolling
```css
div {
  position: fixed;
  top: 0;
}
```

5. `sticky` -> acts like `relative` until a scroll point them becomes fixed 
```css
div {
  position: sticky;
  top: 0;
}
```

---
### ~={purple}9. What is `z-index`?=~
- When and which elements overlap
- Which element comes on top
- Which element goes behind
```CSS
.box1 {
  position: absolute;
  z-index: 1;
}

.box2 {
  position: absolute;
  z-index: 2;
}
```

---
