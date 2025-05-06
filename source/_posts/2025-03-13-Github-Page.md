---
title: Github Page
date: 2025-03-13
tag: Github
---
# Github Page是什么
Github Page是Github提供的一种静态网页托管服务，可以用来展示你的项目、博客等。Github Page支持HTML、CSS、JavaScript等前端技术，并且可以绑定自定义域名。

## 如何使用Github Page
1. 在Github上创建一个新的仓库，仓库名称必须为`yourusername.github.io`，其中`yourusername`是你的Github用户名。
2. 在仓库中创建一个`_config.yml`文件，内容为`theme: minima`（主题）。
3. 在仓库中创建一个`index.md`文件，这是主页。
4. 创建`_post`文件夹，这是用来放md文件的，文件名格式一般为yyyy-mm-dd-文件名.md。文件头如下
```
---
title: "Hello"
date: 2025-03-12
---
```
5. 提交后，setting中找到Github Page，选择分支，部署方式为github action,点击保存即可。

