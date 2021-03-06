# 如何用一行 CSS 分别实现 10 种现代布局

## 01.超级居中

在没有 flex 和 grid 之前，垂直居中一直不能很优雅的实现。而现在，我们可以结合 grid 和 place-items 优雅的同时实现水平居中和垂直居中。

```html
<div class="parent blue">
  <div class="box coral" contenteditable>
    :)
  </div>
</div>
```

```css
.ex1 .parent {
  display: grid;
  place-items: center;
}
```

https://codepen.io/una/pen/YzyYbBx

## 02. 可解构的自适应布局(The Deconstructed Pancake)

这种布局经常出现在电商网站：

在 viewport 足够的时候，三个 box 固定宽度横放
在 viewport 不够的时候（比如在 mobile 上面），宽度仍然固定，但是自动解构（原谅我的中文水平），不在同一水平面上

```html
<div class="parent white">
  <div class="box green">1</div>
  <div class="box green">2</div>
  <div class="box green">3</div>
</div>
```

```css
.ex2 .parent {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

.ex2 .box {
  flex: 1 1 150px; /*  flex-grow: 1 ，表示自动延展到最大宽度 */
  flex: 0 1 150px; /*  No stretching: */
  margin: 5px;
}
```

https://codepen.io/una/pen/WNQdBza

## 03.经典的 sidebar

```html
<div class="parent">
  <div class="section yellow" contenteditable>
    Min: 150px / Max: 25%
  </div>
  <div class="section purple" contenteditable>
    This element takes the second grid position (1fr), meaning it takes up the
    rest of the remaining space.
  </div>
</div>
```

```css
.ex3 .parent {
  display: grid;
  grid-template-columns: minmax(150px, 25%) 1fr;
}
```

https://codepen.io/una/pen/gOaNeWL

## 04.固定的 header 和 footer

grid-template-rows: auto 1fr auto
固定高度的 header 和 footer，占据剩余空间的 body 是经常使用的布局，我们可以利用 grid 和 fr 单位完美实现。

```html
<div class="parent">
  <header class="blue section" contenteditable>Header</header>
  <main class="coral section" contenteditable>Main</main>
  <footer class="purple section" contenteditable>Footer Content</footer>
</div>
```

```css
.ex4 .parent {
  display: grid;
  grid-template-rows: auto 1fr auto;
}
```

https://codepen.io/una/pen/bGVXPWB

## 05. 经典的圣杯布局(classical holy Grail layout)

grid-template: auto 1fr auto / auto 1fr auto
我们可以轻松的使用 Grid 布局来实现圣杯布局，并且是弹性的。

```html
<div class="parent">
  <header class="pink section">Header</header>
  <div class="left-side blue section" contenteditable>Left Sidebar</div>
  <main class="section coral" contenteditable>Main Content</main>
  <div class="right-side yellow section" contenteditable>Right Sidebar</div>
  <footer class="green section">Footer</footer>
</div>
```

```css
.ex5 .parent {
  display: grid;
  grid-template: auto 1fr auto / auto 1fr auto;
}

.ex5 header {
  padding: 2rem;
  grid-column: 1 / 4;
}

.ex5 .left-side {
  grid-column: 1 / 2;
}

.ex5 main {
  grid-column: 2 / 3;
}

.ex5 .right-side {
  grid-column: 3 / 4;
}

.ex5 footer {
  grid-column: 1 / 4;
}
```

https://codepen.io/una/pen/mdVbdBy

## 06.有意思的叠块

使用 grid-template-columns 和 grid-column 可以实现如下图所示的布局。进一步说明了 repeat 和 fr 的便捷性。

https://codepen.io/una/pen/eYJOYjj

## 07. RAM 技巧

grid-template-columns: repeat(auto-fit, minmax(<base>, 1fr))

Una Kravets 称之为 repeat, auto, minmax 技巧。这在弹性布局 图片/box 这类非常有用(一行可以排放的卡片数量自动适应)。

```css
.ex7 .parent {
  display: grid;
  grid-gap: 1rem;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
}
```

```html
<div class="parent white">
  <div class="box pink">1</div>
  <div class="box purple">2</div>
  <div class="box blue">3</div>
  <div class="box green">4</div>
</div>
```

复制代码其效果是如果能够满足多个 box 的最小宽度（比如上面的 150px），自动弹性适应放在多行。

举个例子:

- 当前宽度是 160px（不考虑 gap），那么上面四个 box 的宽度会自适应为 160px,并且分为 4 行
- 当前宽度是 310px （不考虑 gap），上面四个 box 分成两行，两个 box 平分宽度
- 当满足一行放下 3 个 box 时，第三个 box 自动到第一行
- 当满足一行放下 4 个 box 时，第四个 box 自动到第一行

## 08. 卡片弹性自适应

justify-content: space-between，结合 grid 和 flex 实现完美的卡片布局。

```html
<div class="parent white">
  <div class="card yellow">
    <h3>Title - Card 1</h3>
    <p contenteditable>Medium length description with a few more words here.</p>
    <div class="visual pink"></div>
  </div>
  <div class="card yellow">
    <h3>Title - Card 2</h3>
    <p contenteditable>
      Long Description. Lorem ipsum dolor sit, amet consectetur adipisicing
      elit.
    </p>
    <div class="visual blue"></div>
  </div>
  <div class="card yellow">
    <h3>Title - Card 3</h3>
    <p contenteditable>Short Description.</p>
    <div class="visual green"></div>
  </div>
</div>
```

```css
.ex8 .parent {
  height: auto;
  display: grid;
  grid-gap: 1rem;
  grid-template-columns: repeat(3, 1fr);
}

.ex8 .visual {
  height: 100px;
  width: 100%;
}

.ex8 .card {
  display: flex;
  flex-direction: column;
  padding: 1rem;
  justify-content: space-between;
}

.ex8 h3 {
  margin: 0;
}
```

https://codepen.io/una/pen/ExPYomq

## 09.使用 clamp 实现 fluid typography

clamp(<min>, <actual>, <max>)

使用最新的 clamp() 方法可以一行实现 fluid typography。提高 UX，非常适合包含阅读内容的 card，因为我们不希望一行字太短或太长。

```html
<div class="parent white">
  <div class="card purple">
    <h1>Title Here</h1>
    <div class="visual yellow"></div>
    <p>
      Descriptive Text. Lorem ipsum dolor sit, amet consectetur adipisicing
      elit. Sed est error repellat veritatis.
    </p>
  </div>
</div>
```

```css
.ex9 .parent {
  display: grid;
  place-items: center;
}

.ex9 .card {
  width: clamp(23ch, 50%, 46ch);
  display: flex;
  flex-direction: column;
  padding: 1rem;
}

.ex9 .visual {
  height: 125px;
  width: 100%;
}
```

https://developer.mozilla.org/en-US/docs/Web/CSS/clamp

## 10. 完美实现比例

aspect-ratio: <width> / <height>
在展现 CMS 或其他设计内容时，我们会期望图片、video、卡片能够按照固定的比例进行布局。而最新的 aspect-ratio 就能优雅的实现该功能(使用 chrome 84+)

```html
<div class="parent white">
  <div class="card blue">
    <h1>Video Title</h1>
    <div class="visual green"></div>
    <p>Descriptive Text. This demo works in Chromium 84+.</p>
  </div>
</div>
```

```css
.ex10 .parent {
  display: grid;
  place-items: center;
}

.ex10 .visual {
  aspect-ratio: 16 / 9;
}

.ex10 .card {
  width: 50%;
  display: flex;
  flex-direction: column;
  padding: 1rem;
}
```

https://codepen.io/una/pen/xxZKzaX

## 参考链接

https://juejin.im/post/5f058bef6fb9a07eac0665fb
