[English Version](https://github.com/calidion/egg/edit/master/README.md)
[中文版](https://github.com/calidion/egg/edit/master/README.zh-CN.md)

# 蛋蛋 API: 一个Nodejs的简单API规范
蛋蛋API是一个更简单的基于HTTP的API规则。
蛋蛋API的目标是降低RESTful API的复杂性

1. 蛋蛋API只将HTTP的GET/POST方法作为基本方式.
2. GET方法用于从服务器获取数据.
3. POST方法用于向服务器发送数据.
4. 任何使用GET方法的API将不会改变除日志之外的用户信息.
5. 任何使用POST方法的API将会改变用户的状态，信息或者数据
6. 所有使用GET方法的API必须以英文名词开关，比如user/admin.  
  * 只写一个名词表示你需要获取到所有的信息
    > GET /user
  * 默认分页的大小是20个数据，如果你想修改这个默认值，可以添加limit的值，格式如下:
    > GET /user?limit=50
  * 默认页面是第一页，如果你想修改当前的页面数，可以指定page值，格式如下:
    > GET /user?page=3
  * 在名词后接一个数值或者一个ID可以获得一个数据项，这时limit与page无效，会被忽略。
    > GET /user/1  
    > GET /user/1?limit=50&page=10

7. 所有使用POST方法的API必须以英文动词开头，比如可以修改数据的create/update/remove，但是不能是get/list/show这样的不会修改数据的动词。
    简单示例如下:
    > POST /create/user  
    > POST /update/user  
    > POST /remove/user  

8. POST格式需要跟表单提交的格式相同。禁止提交json/xml文件。
    > POST /create/user
    > ...
    > 
    > 
    > name=aaa&password=asdfsf
9. 所以的API返回JSON数据。
10. 所有的JSON数据包括以下字段：:

    | 字段名 | 描述 |
    | --- | --- |
    | code | 错误代码|
    | message | 错误消息|
    | data | 返回数据 |

11. 错误代在[errorable](https://github.com/calidion/errorable)和它的[common](https://github.com/Errorable/common)库定义.
