# iconfont

>在[iconfont](http://iconfont.cn/)上选择生成自己的业务图标库

## 生成图标库代码

首先，搜索并找到你需要的图标，将它采集到你的购物车里，在购物车里，你可以将选中的图标添加到项目中（没有的话，新建一个），后续生成的资源/代码都是以项目为维度的

>如果你已经有了设计稿，只是需要生成相关代码，可以上传你的图标后，再进行上面的操作

## vue中使用方式

配置webpack，插件[svg-sprite-loader](http://blog.csdn.net/zb0567/article/details/77987727?locationNum=9&fps=1)

推荐单独导出svg格式的文件，引入使用

下载完成后，将.svg文件放入到`@/components/icons-svg/svg`目录下，就会自动导入

在实际项目中，写成一个公共的组件 `@/components/icons-svg/index.vue`，并且在`main.js`中进行全局注册
~~~ template
<template>
  <svg class="svg-icon" aria-hidden="true">
    <use :xlink:href="iconName"></use>
  </svg>
</template>
~~~

在js部分利用webpack的**require.context**实现[“去中心化”管理](https://www.v2ex.com/t/359380)
~~~js
const requireAll = requireContext => requireContext.keys().map(requireContext);
const req = require.context('./svg', false, /\.svg$/);
requireAll(req);
~~~

全局配置[icon的样式](http://iconfont.cn/help/detail?spm=a313x.7781069.1998910419.14&helptype=code)
~~~css
.svg-icon {
  width: 1em;
  height: 1em;
  vertical-align: -0.15em;
  fill: currentColor;
  overflow: hidden;
}
~~~

使用方式
~~~template
<svg-icon icon-class="password" /> //icon-class 为 icon 的名字
~~~

## 改变颜色

`svg-icon` 默认会读取其父级的 color `fill: currentColor;`

我们可以改变父级的`color` 或者 直接修改`fill`的颜色即可
