---
title: "在Vue中svg的使用"
date: 2020-04-29T15:29:46+08:00
draft: false
categories:
  - Vue svg webpack
tags:
  - Vue svg webpack
featured_image:
---

如何在 Vue 项目中引入 svg：

1：在 src/assets/下建立一個 icons 文件夾，將下載的 svg 圖片放在這裏

2：引入 svg-sprite-loader，并在 vue.config.js 中进行配置；

這樣就可以通過

```
import 'src/assets/icons/XXX.svg',

<svg><use xlink:href="#XXX"></use></svg>
```

來引入使用這個圖片

3：上面有兩點需要改進的地方
第一是每次需要引入一個 svg 圖片，就需要 import 'src/assets/icons/\*\*\*\*.svg',
第二個是每次使用的時候，都要寫

```
<svg><use xlink:href="#\*\***"></use></svg>
```

这句代码。

針對第一個問題：使用 webpack 的 require.context,通过正则匹配引入相应的文件模块。

針對第二個問題：抽離出一個組件，專門用來顯示圖標。所以可以建立一個 Icon 的組件，在裏面使用 require.context 使得相应目录下的圖標全部自動加載，同時使用圖標的時候，引入 Icon 組件，使用該組件即可。

在 vue-config.js 中的配置要注意：我們需要處理的 svg 圖片僅在 src/assets/icons，這個路徑下面的圖片也不需要被其他 loader 處理了。其他路徑的圖片也不需要用 svg-sprite-loader 去處理。同時如果需要對 svg 圖片進行優化，也可以使用 svgo 去優化。

關於 svg 圖片的使用具體可以參考[Vue 項目中優雅使用 svg](https://juejin.im/post/5dd00735f265da0be6509c53#heading-19)以及[手摸手，帶你優雅的使用 icon](https://juejin.im/post/59bb864b5188257e7a427c09#heading-8)這兩篇文章
