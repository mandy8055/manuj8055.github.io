---
title: Applying background images and videos on websites
tags: [CSS, machine-coding]
description: Learning how to apply images and videos as website background
mathjax: false
style: fill
color: danger
---

In this post I'll share my learning on how to apply images and videos as website background. Along with that how can we apply overlay so that the imperfections(pixel fading, etc.) inside video can be hidden.

### Working with the HTML

For applying the video background there will be requirement of below HTML:

```html
<video class="video-background" muted autoplay loop>
  <source src="your_video.mp4" type="video/mp4" />
</video>
<div class="video-overlay"></div>
```

Similarly, for image element:

```html
<div class="img-container">
  <img src="src_of_img" alt="image background" />
</div>
<div class="overlay"></div>
```

### The CSS

CSS required for the video:

```css
body {
  margin: 0;
  min-height: 100vh;
  overflow-y: hidden;
  display: flex;
  align-items: center;
}

.video-background {
  position: fixed;
  right: 0;
  bottom: 0;
  width: 100vw;
  height: auto;
}

video {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.video-overlay {
  position: fixed;
  left: 0;
  top: 0;
  height: 100vh;
  width: 100vw;
  background: rgba(255, 255, 255, 0.35);
}
```

Similarly, the CSS for the image:

```css
body {
  margin: 0;
  min-height: 100vh;
  overflow-y: hidden;
  display: flex;
  align-items: center;
}
.img-container {
  position: fixed;
  right: 0;
  bottom: 0;
  width: 100vw;
  height: auto;
}

.overlay {
  position: fixed;
  left: 0;
  top: 0;
  height: 100vh;
  width: 100vw;
  background: rgba(255, 255, 255, 0.35);
}
```
