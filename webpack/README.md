### webpack简介

`webpack`是一个前端模块化打包工具，最开始它只能打包JS文件，但是随着webpack的发展，他还能打包如CSS、图片等文件。主要由入口，出口，loader，plugins四个部分。 处理模块间的依赖关系，并把他们进行打包，webpack主要适用场景是单页面富应用

### [模块化](模块化.md)

### [安装](安装.md)

### [webpack基本配置文件](webpack配置文件.md)

### [Loader](loader.md)

### [package-lock.json](./package-lock.json.md)



assets和static

1. 一般assets用于放置可能变动的资源，static放置不会变动的文件
2. assets被webpack处理解析为模块依赖，使用相对资源路径，而static使用绝对路径，如/static/image/...