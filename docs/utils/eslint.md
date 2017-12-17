# eslint

eslint代码规范 + VS Code编辑器

## 安装eslint扩展
ESLint Integrate ESLint into VS Code

## VS Code设置
~~~ json
"eslint.autoFixOnSave": true,
"eslint.validate": [
    "javascript",
    "javascriptreact",
    "html",
    {
        "language": "vue",
        "autoFix": true
    }
],
"eslint.options": {
  "plugins": ["html"]
}
~~~

## 取消ESLint
在**build/webpack.base.conf.js**中注释以下代码
~~~js
{
  test: /\.(js|vue)$/,
  loader: 'eslint-loader',
  enforce: "pre",
  include: [resolve('src'), resolve('test')],
  options: {
      formatter: require('eslint-friendly-formatter')
  }
}
~~~