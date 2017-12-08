# docs
![logo](img1.jpg)
[baidu](http://www.baidu.com)
{docsify-updated} 
# Vue 介绍

`v-for` 的用法

```javascript
<ul>
  <li v-for="i in 10">{{ i }}</li>
</ul>
```

<ul>
  <li v-for="i in 10">{{ i }}</li>
</ul>

# Vuep 使用

<vuep template="#example"></vuep>

<script v-pre type="text/x-template" id="example">
  <style>
      div {
          background-color: red;
      }
  </style>
  
  <template>
    <div>Hello, docsify {{ name }}! dzh1104</div>
  </template>

  <script>
    module.exports = {
      data: function () {
        return { name: 'Vue' }
      }
    }
  </script>
</script>