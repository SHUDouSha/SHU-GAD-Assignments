---
layout: cover
theme: ./
canvasWidth: 1920
canvasHeight: 1080
---

<template v-slot:background-text>
  <span class="font-background-text">COVER</span>
</template>

<span class="font-eyebrow-title">EYEBROW TEXT</span>

<span class="font-title-master">Cover Title</span>

<span class="font-tagline">This is a tagline</span>

---

---
layout: section
theme: ./
canvasWidth: 1920
canvasHeight: 1080
---

<template v-slot:background-text>
  <span class="font-background-text">SECTION</span>
</template>

<span class="font-eyebrow-title">SECTION EYEBROW</span>

<span class="font-title-section">Section Title</span>

<span class="font-tagline">This is a section tagline</span>

---

---
layout: page-standard
theme: ./
canvasWidth: 1920
canvasHeight: 1080
---

<template v-slot:background-text>
  <span class="font-background-text">PAGE</span>
</template>

<span class="font-eyebrow-page">PAGE EYEBROW</span>

<span class="font-title-page">Page Title</span>

<div class="container-textbox-plain">
  <span class="font-body">这是正文内容第一行<br>这是第二行内容<br>这是第三行内容</span>
</div>

---

---
layout: page-two-columns
theme: ./
canvasWidth: 1920
canvasHeight: 1080
---

<template v-slot:background-text>
  <span class="font-background-text">TWO COLUMNS</span>
</template>

<span class="font-eyebrow-page">TWO COLUMNS EYEBROW</span>

<span class="font-title-page">Two Columns Title</span>

<div class="container-textbox-plain">
  <span class="font-body">左列内容第一行<br>左列第二行内容<br>左列第三行内容</span>
</div>

<div class="container-textbox-gray-border">
  <span class="font-body">右列内容第一行<br>右列第二行内容<br>右列第三行内容</span>
</div>

---

---
layout: page-image-text
theme: ./
canvasWidth: 1920
canvasHeight: 1080
---

<template v-slot:background-text>
  <span class="font-background-text">IMAGE TEXT</span>
</template>

<span class="font-eyebrow-page">IMAGE TEXT EYEBROW</span>

<span class="font-title-page">Image Text Title</span>

<ImagePlayer src="./image/photo.png" />

<div class="container-textbox-plain">
  <span class="font-body">这是右列文本内容第一行<br>这是第二行内容<br>这是第三行内容</span>
</div>

---
