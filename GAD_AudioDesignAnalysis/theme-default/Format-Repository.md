# Format-Repository

## 页面全局

- 页面左边距：--global-padding-left：3.8cm
- 页面右边距：--global-padding-right：3.8cm
- 页面上边距：--global-padding-top：7.92cm
- 页面下边距：--global-padding-bottom：3cm
- 页面背景色：--global-bg-color：rgb(10, 10, 10)
- 画布宽度：--canvas-width：50.8cm
- 画布高度：--canvas-height：28.575cm

### 背景字

- 对齐方式：text-align
- 背景字颜色RGB：color
- 背景字字体：font-family
- 背景字字号：font-size

```
.font-background-text {
  font-family: 'Times New Roman', serif;
  font-size: 390pt;        /* 换算为 520px */
  color: rgb(20, 20, 20);
  text-align: center;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  white-space: nowrap;
  opacity: 0.3;
  pointer-events: none;
}
```



### 字体样式

- 字体：font-family
- 字号：font-size
- 颜色：color
- 加粗：font-weight
- 下划线：text-decoration
- 下划线颜色：text-decoration-color
- 下划线距离：text-underline-offset
- 行距：line-height
- 上边距：top

```
/* ========== 字体样式 ========== */

/* 全文唯一标题 */
.font-title-master {
  font-family: 'Noto Sans SC', sans-serif;
  font-size: 96pt;          /* 换算为 128px */
  color: rgb(240, 240, 240);
  font-weight: bold;
  line-height: 1.5;
}

/* 段落标题 */
.font-title-section {
  font-family: 'Noto Sans SC', sans-serif;
  font-size: 74pt;          /* 换算为 98.6px */
  color: rgb(240, 240, 240);
  font-weight: bold;
  line-height: 1.5;
}

/* 页标题 */
.font-title-page {
  font-family: 'Noto Sans SC', sans-serif;
  font-size: 60pt;          /* 换算为 80px */
  color: rgb(240, 240, 240);
  font-weight: bold;
  line-height: 1.5;
  position: absolute;
  top: 4.4cm;               /* 换算为 166.3px */
}

/* 主标题上方的小字（引导文字） */
.font-eyebrow-title {
  font-family: 'Times New Roman', serif;
  font-size: 20pt;          /* 换算为 27px */
  color: rgb(80, 80, 80);
  font-weight: normal;
  text-decoration: underline;
  text-decoration-color: rgb(212, 90, 42);
  text-underline-offset: 6pt;  /* 换算为 8px */
  line-height: 1.5;
}

/* 主标题下方的一句话概括（标语） */
.font-tagline {
  font-family: 'Noto Sans SC', sans-serif;
  font-size: 27pt;          /* 换算为 36px */
  color: rgb(170, 170, 170);
  font-weight: normal;
  line-height: 1.5;
}

/* 强调 */
.font-highlight {
  font-family: 'Noto Sans SC', sans-serif;
  font-size: 34pt;          /* 换算为 45px */
  color: rgb(240, 240, 240);
  font-weight: bold;
  line-height: 1.5;
}

/* 正文 */
.font-body {
  font-family: 'Noto Sans SC', sans-serif;
  font-size: 21pt;          /* 换算为 28px */
  color: rgb(170, 170, 170);
  font-weight: normal;
  line-height: 1.5;
}

/* 页标题上方的小字 */
.font-eyebrow-page {
  font-family: 'Times New Roman', serif;
  font-size: 20pt;          /* 换算为 27px */
  color: rgb(80, 80, 80);
  font-weight: normal;
  text-decoration: underline;
  text-decoration-color: rgb(212, 90, 42);
  text-underline-offset: 6pt;  /* 换算为 8px */
  line-height: 1.5;
  position: absolute;
  top: 2.5cm;               /* 换算为 94.5px */
}

/* 标签 */
.font-tag {
  font-family: 'Times New Roman', serif;
  font-size: 12pt;          /* 换算为 16px */
  color: rgb(212, 90, 42);
  font-weight: bold;
  line-height: 1.5;
}
```



### 容器样式（保留原始单位，生成时换算）

- 背景色RGB：background-color
- 边框颜色RGB：border-color
- 边框线条宽度：border-width
- 容器内边距（上）：padding-top
- 容器内边距（下）：padding-bottom
- 容器内边距（左）：padding-left
- 容器内边距（右）：padding-right
- 上边距：top

```
/* ========== 容器样式 ========== */

/* 文本框（无色无边框） */
.container-textbox-plain {
  background-color: transparent;
  border: none;
  padding: 0.13cm 0.25cm 0.13cm 0.25cm;  /* 换算为 4.9px 9.5px 4.9px 9.5px */
  position: absolute;
  top: 7.9cm;                             /* 换算为 298.6px */
}

/* 文本框（灰色灰边框） */
.container-textbox-gray-border {
  background-color: rgb(20, 20, 20);
  border: 0.75pt solid rgb(40, 40, 40);  /* 换算为 1px solid rgb(40, 40, 40) */
  padding: 0.13cm 0.25cm 0.13cm 0.25cm;  /* 换算为 4.9px 9.5px 4.9px 9.5px */
  position: absolute;
  top: 7.9cm;                             /* 换算为 298.6px */
}
```



### 组件样式（不使用 CSS 变量）

