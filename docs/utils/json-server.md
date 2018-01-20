## json-server

借助json-server这个工具快速搭建后台管理系统的服务端程序:

- 首先执行npm i json-server -g把json-server作为全局工具安装
- 新建/server目录用于放置服务端的文件
- 新建/server/db.json文件作为服务端的数据库文件
- 在/server/db.json文件中写入以下数据:

```json
{
  "user": [
    {
      "id": 10000,
      "name": "一韬",
      "age": 25,
      "gender": "male"
    },
    {
      "id": 10001,
      "name": "张三",
      "age": 30,
      "gender": "female"
    }
  ],
  "book": [
    {
      "id": 10000,
      "name": "JavaScript从入门到精通",
      "price": 9990,
      "owner_id": 10000
    },
    {
      "id": 10001,
      "name": "Java从入门到放弃",
      "price": 1990,
      "owner_id": 10001
    }
  ]
}
```

- 最后在/server目录执行json-server db.json -w -p 3000

我们在db.json文件中写入了标准的JSON格式数据，这个json里有一个user数组和一个book数组，这就告诉json-server，我们的数据库里有一个名为user的”表”和一个名为book的”表”，并且表里的数据为xxx，然后json-server就会启动服务器，并且以每个”表”为单位为我们注册一系列标准的RESTFull形式的API接口（路由），以user为例:

- [GET] /user #获取用户列表的接口
- [GET] /user/:id #获取单个用户的接口
- [POST] /user #新增用户的接口
- [PUT] /user/:id #修改用户的接口
- [DELETE] /user/:id #删除用户的接口

在获取列表的接口中也可以追加查询参数，来限定查询的结果，比如:

- 查询所有男性用户： /user?gender=male
- 查询年龄大于等于20并且小于等于29的用户：/user?age_gte=20&age_lte=29

此外还有分页、排序、匹配、关系查询等查询方式，可以在[这里查看更多](https://github.com/typicode/json-server#routes)