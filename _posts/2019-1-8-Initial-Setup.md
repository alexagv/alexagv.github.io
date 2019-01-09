---
layout: post
title: Jekyll Now Initial Setup
---
Sharing my initial setup after the "30 second quick start"

## Fixing code snippets bug
Code snippets will appear like two concentric boxes without this [fix](https://github.com/barryclark/jekyll-now/issues/1055):  
`_sass/_highlights.scss`
```css
.highlight {
  ...
  margin: 0px 0 0px 0;
  overflow: auto;
}

.highlight .highlight {
  border: none;
  box-shadow: none;
}
```

## Adding a favicon
Convert your avatar/logo to a 16x16 .ico named `favicon.ico` and upload the favicon.ico to the root folder of your jekyll-now repo.  
Then modify `_layouts/default.html` by inserting the code below somewhere in-between the `<head> </head>` tags.
```html
<link rel="shortcut icon" href="/favicon.ico" type="image/x-icon" />
```
