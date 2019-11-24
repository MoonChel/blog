---
title: Yet another new tab editor
description: We need more chrome extensions, MORE!
date: 2019-11-23T23:00:00Z

---
Last week I was looking for chrome extension, which will support some simple notes management in new browser tab, plus markdown editor. 

I failed. I hate to fail.

Here is my version of notes editor in new tab.

## Available features:

1. Markdown support - thanks to [tiptap](https://tiptap.scrumpy.io/ "tiptap")
2. Notes management - thanks to [vuex](https://vuex.vuejs.org/ "vuex") & [vuex-persist](https://github.com/championswimmer/vuex-persist "vuex persist")
3. Common, it's just nice - thanks to [chota](https://jenil.github.io/chota/ "chota")

## Development

I used a small trick to compile this [vuejs](https://vuejs.org/ "vuejs") project. Basically you need to have a manifest file inside your extension and all `js` files should be minified and exported into 1 main `app.js`. So, after `webpack` build your `dist` folder is basically your working extension, all you need is to add correct `manifest.js`. 

You can check it here:

[https://github.com/MoonChel/yet-another-new-tab-editor](https://github.com/MoonChel/yet-another-new-tab-editor "https://github.com/MoonChel/yet-another-new-tab-editor")