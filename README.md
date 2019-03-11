# caishijian2008.github.io
- 本Github Page 博客，基于Hexo。
- 关于Hexo的使用，可参考官方网站文档[hexo.io](https://hexo.io/zh-cn/docs/)

## 安装与使用
``` bash
git clone https://github.com/caishijian2008/mygithubpage.git Hexo
cd Hexo
npm install
hexo clean  （清除已生成的静态文件）
hexo generate  /  hexo g (生成的静态文件。-d: 文件生成后立即部署网站, -w: 监视文件变动)
hexo deploy  /  hexo d  （部署）
浏览器访问 https://caishijian2008.github.io/

PS：
hexo g -d   // 生成静态文件并立即部署
```

## 发布文章
``` bash
hexo new [layout] <title>
```
eg.
``` bash
hexo new post 'My First Post'
OR
hexo new 'My First Post'
```
