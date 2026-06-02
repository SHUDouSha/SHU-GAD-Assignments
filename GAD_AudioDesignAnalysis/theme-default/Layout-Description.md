# Layout-Description

## 项目构建要求

### 基础要求

- 请阅读并记住 `./Format-Repository.md` 文件中的所有 CSS 类定义（如 `.font-title-master`、`.font-eyebrow-title`、`.container-textbox-plain` 等）。
- 下文创建 Layout 的 Prompt 中会引用这些类名，请确保生成的 Layout 中的代码使用它们。
- 下文创建 Layout 的 Prompt 中会引用其中`--global-padding-left`等定值，请确保生成的 Layout 中的代码直接使用其对应的转换后的值
- 将所有以 cm 为单位的数值按 **1cm = 37.8px** 换算为 px，所有以 pt 为单位的数值按 **1pt = 1.333px** 换算为 px。最终生成的 CSS 样式定义中，所有间距、字号、边距等长度单位统一使用 px，不得保留 cm 或 pt。
- 由于 Slidev 使用 Shadow DOM，CSS 变量（如 `var(--xxx)`）无法穿透，所有样式必须直接写具体的数值（px），不能使用 CSS 变量。
- 不要自行创建新的命名或硬编码样式值。

### 路径要求

- 所有页面，背景字，字体，容器的样式定义放在`./styles/layouts.css`，使用可复用的类名
- 所有音频，视频，图片组件的定义放在 `./components` 下
- 所有 Layout 的定义放在 `./layouts` 下
- 完成所有 Layout 的创建后，在 `./` 下创建测试用例 slide.md，并在 headmatter中添加以下字段

```
---
layout: cover
theme: ./
canvasWidth: 1920
canvasHeight: 1080
---
```

## 样式创建

**基础要求**

- 请根据已阅读的字体样式定义

- 关于字体的修改，请同步至 package.json 文件。

**Shadow DOM 兼容要求：**

- 所有颜色直接写 RGB 值（如 `rgb(10, 10, 10)`），不使用 CSS 变量
- 所有尺寸直接写 px 值（如 `143.6px`），不使用 CSS 变量
- 样式表不需要 `:root` 定义变量

**示例输出格式：**

```
/* ========== 全局样式 ========== */
/* 注意：不使用 :root，直接定义类样式 */

/* 全文唯一标题 */
.font-title-master {
  font-family: 'Noto Sans SC', sans-serif;
  font-size: 128px;  /* 96pt × 1.333 = 128px */
  color: rgb(240, 240, 240);
  font-weight: bold;
  line-height: 1.5;
}
```



## 组件创建

**基础要求**

- 请创建三个 Vue 3 组件：`VideoPlayer.vue`、`AudioPlayer.vue`、`ImagePlayer.vue`
- 这些组件将在 `.md` 文件中被直接调用
- 调用时只需传入 `src` 参数说明资源路径，尺寸和位置由 Layout 的 CSS 预先定义
- 示例：`.md` 文件中写 `<VideoPlayer src="/videos/demo.mp4" />`

- 组件根元素使用 `.component-video-player`、`.component-audio-player`、`.component-image-player` 类名
- 组件不写死任何 width、height、top、left（这些由 Layout 的 CSS 控制）
- 组件根元素默认 `position: absolute`，以便通过 CSS 定位

- 接收 `src` prop（字符串，必需）
- 接收 `autoplay`、`controls`、`loop` 等布尔 prop（controls 默认 true，autoplay 默认 false）
- 接收 `customClass` prop（可选，用于添加额外类名）
- **不接收** `width`、`height`、`top`、`left` props（这些由 CSS 控制，不在调用时传入）
- 使用原生 HTML 元素：`<video>`、`<audio>`、`<img>`
- 对于图片，视频组件，确保播放时无论素材尺寸如何，其能在组件中完整的显示

**Shadow DOM 兼容要求：**

- 使用 `<style>` 而不是 `<style scoped>`
- 组件不能使用 CSS 变量

**示例输出格式：**

```vue
<template>
  <div 
    :class="['component-video-player', customClass]"
  >
    <video :src="src" :autoplay="autoplay" :controls="controls" :loop="loop" />
  </div>
</template>

<script setup lang="ts">
interface Props {
  src: string
  autoplay?: boolean
  controls?: boolean
  loop?: boolean
  customClass?: string
}

withDefaults(defineProps<Props>(), {
  autoplay: false,
  controls: true,
  loop: false,
  customClass: ''
})
</script>

<style>
.component-video-player {
  position: absolute;
}
</style>
请输出三个组件的完整代码。
```

## Layout 创建

**基础要求**

- 为所有页面采用 `--global-bg-color`作为背景色。
- 若layout里使用了容器，其尺寸的定义在对应的layout中根据prompt在对应的.vue文件中定义，不要在.css文件中直接定义
- 背景字容器必须独立于内容层，使用命名 slot <slot name="background-text" />
- 背景字容器设置 z-index: 0，确保在最底层
- 内容容器设置 position: relative 和 z-index: 1，确保在最顶层

- 文本框内换行方法：在 span 内使用 HTML 换行标签` <br>`

```
<span class="font-body">第一行内容<br>第二行内容<br>第三行内容</span>
```

**shadow DOM 兼容要求：**

- 使用 `<style>` 而不是 `<style scoped>`
- 字体样式通过 `<span class="font-xxx">` 类名控制
- 容器使用 `<div class="container-xxx">` 包裹内容
- 组件直接使用 `<ComponentName />` 调用
- Layout 只保留默认 slot `<slot />`

**示例输出格式**

```
/* page-image-text.vue */
<script setup lang="ts">
defineProps<{
  background?: string
}>()
</script>

<template>
  <div
    class="slidev-layout page-image-text"
    :style="background ? { backgroundColor: background } : {}"
  >
    <div class="page-image-text-background-text">
      <slot name="background-text" />
    </div>
    <div class="page-image-text-content">
      <slot />
    </div>
  </div>
</template>

<style>
.slidev-layout.page-image-text {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100%;
  background-color: rgb(10, 10, 10);
  position: relative;
}

.page-image-text-background-text {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 0;
  pointer-events: none;
}

.page-image-text-content {
  position: relative;
  z-index: 1;
  display: flex;
  flex-direction: column;
  padding-top: 300px;
  padding-bottom: 113px;
  padding-left: 144px;
  padding-right: 144px;
  width: 100%;
  height: 100%;
  box-sizing: border-box;
  gap: 0;
}

/* 图片 - 左列 */
.page-image-text-content .component-image-player {
  position: absolute;
  top: 299px;
  left: 144px;
  width: 1075px;
  height: 667px;
  box-sizing: border-box;
}

/* 文本框 - 右列 */
.page-image-text-content .container-textbox-plain {
  position: absolute;
  top: 299px;
  left: 1239px;
  width: 537px;
  height: 667px;
  box-sizing: border-box;
}
</style>
```

**示例应用格式**

```
/* slide.md */
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

```



### cover.vue

 请帮我创建此 Layout，要求如下：

  **布局要求：**

  - `.font-title-master`：上下居中，左边距 --global-padding-left
  - `.font-eyebrow-title`：位于 `.font-title-master` 上方，距离由行距自然决定，左边距 --global-padding-left
  - `.font-tagline`：位于 `.font-title-master` 下方，距离由行距自然决定，左边距 --global-padding-left

- `.font-background-text`



  ### section.vue

  请帮我创建此 Layout，要求如下：

**布局要求：**

- `.font-title-section`:上下居中，左右居中
- `.font-eyebrow-title`：位于`.font-title-section`上方，距离由行距自然决定，左右居中
- `.font-tagline`：位于`.font-title-section`下方，距离由行距自然决定，左右居中



### page-standard.vue

  请帮我创建此 Layout，要求如下：

**布局要求：**

- `.font-eyebrow-page`

- `.font-title-page`
- `.container-textbox-plain`:其上边距为`--global-padding-top`,下边距为`--global-padding-bottom`,左右居中，左边距为`--global-padding-left`,右边距为`--global-padding-right`
- `.font-background-text`

**Note:**

- `.container-textbox-plain`的上，下，左，右边距均是相对页面而言。



### page-two-columns.vue

  请帮我创建此 Layout，要求如下：

**布局要求：**

- `.font-eyebrow-page`

- `.font-title-page`

- `.container-textbox-plain`:其上边距为`--global-padding-top`,下边距为`--global-padding-bottom`,左边距为`--global-padding-left`,
- `.container-textbox-gray-border`:其上边距为`--global-padding-top`,下边距为`--global-padding-bottom`,右边距为`--global-padding-right`
- `.font-background-text`

**Note:**

- `.container-textbox-plain`和`.container-textbox-gray-border`中间间隔20px,`.container-textbox-plain`在左，`.container-textbox-gray-border`在右.其宽度比为1：1由此计算并两个容器的尺寸
- 将`.container-textbox-gray-border`的左边框线更改为rgb(212, 90, 42)



### page-image-text.vue

  请帮我创建此 Layout，要求如下：

**布局要求：**

- `.font-eyebrow-page`

- `.font-title-page`

- `image-player`:其上边距为`--global-padding-top`,下边距为`--global-padding-bottom`,左边距为`--global-padding-left`,

- `.container-textbox-plain`:其上边距为`--global-padding-top`,下边距为`--global-padding-bottom`,右边距为`--global-padding-right`
- `.font-background-text`

**Note:**

`image-player`和`.container-textbox-plain`中间间隔20px.`image-player`在左，`.container-textbox-plain`在右，其宽度比为2：1.由此计算`image-player`和`.container-textbox-plain`的尺寸
