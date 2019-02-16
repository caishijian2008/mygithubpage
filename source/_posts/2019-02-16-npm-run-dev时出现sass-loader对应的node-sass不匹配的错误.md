---
title: npm run dev时出现sass-loader对应的node-sass不匹配的错误
date: 2019-02-16 14:46:49
updated: 2019-02-16 14:46:49
tags: node.js, sass
categories: CSS
---

npm run dev时会出现sass-loader对应的node-sass不匹配的错误，这是因为
sass-loader，node-sass和webpack要成对出现的，所以：（查看npm.js中[sass-loader](https://www.npmjs.com/package/sass-loader)的安装说明）
``` bash
npm install sass-loader node-sass webpack --save-dev
```
