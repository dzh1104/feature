# webpack

在项目中，使用的webpack配置用于优化或提高开发效率

## require.context

使用webpack的**require.context**实现[`去中心化管理`](https://www.v2ex.com/t/359380)

>使用场景: 在路由文件、svg文件等引入，做到只需添加相应的文件即可，无需再用文件名单独引入

## require.ensure

[`code split`](https://cnodejs.org/topic/586823335eac96bb04d3e305) 将一些js模块给独立出一个个js文件，然后需要用到的时候，再创建一个script对象，加入到document.head对象中即可，浏览器会自动去请求这个js文件，再写个回调，去定义得到这个js文件后，需要做什么业务逻辑操作

webpack的强大功能之一: [`require.ensure`](https://doc.webpack-china.org/guides/migrating/#require-ensure-amd-require-)是一个代码分离的分割线，表示回调里的require是我们要分割出去的，即require('vendor/Export2Zip')，把其分割出去，形成一个webpack打包的单独文件

不是在组件初始化挂载就引入，只有用户点击导出zip才会加载相应的依赖，点击后可以查看到index.html中head加了一个script标签

第一个参数[] 是一个数组，当前这个 require.ensure所依赖的其他 异步加载的模块

## dynamic import()

>Instead of downloading the entire app before users can use it, code splitting allows you to split your code into small chunks which you can then load on demand

`require.ensure`是webpack的过渡产物，不推荐使用，应当使用[dynamic import()](https://github.com/dzh1104/dzh-react#code-splitting)

>The import() function-like form takes the module name as an argument and returns a Promise which always resolves to the namespace object of the module.