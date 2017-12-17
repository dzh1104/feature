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