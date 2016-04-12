[[中文版](https://github.com/calidion/egg/blob/master/README.md)]
[[English Version](https://github.com/calidion/egg/blob/master/README.end.md)]


# 蛋蛋 API: 一个Nodejs的简单的类RESTful API规范

蛋蛋API是一个更简单的基于HTTP的API规则。
蛋蛋API的目标是降低RESTful API的复杂性。

## RESTful的问题
1. 使用过多的HTTP方法，很多方法大部分程序员并不熟悉
2. 虽然使用了很多的HTTP方法，但是这些方法跟现实比又显得不够用。
3. PUT/POST等方法语义接近，区分困难
4. RESTful不容易理解，大部分程序员没有真正的理解RESTful API
5. 过于学术化不实用

## 为什么要定义蛋蛋API？

1. 降低门槛，简单化
2. 更加符合实际，好操作，好使用，更接近人的使用直觉
3. 吸引RESTful里精华的部分
4. 去掉RESTful里不实用的部分
5. 可以自定义更多的动作
6. 吸收RESTful无状态，资源定位等理念的优点

## 蛋蛋API规范

1. 蛋蛋API只将HTTP的GET/POST方法作为基本方式.
2. GET方法用于从服务器获取数据.
3. POST方法用于向服务器发送数据.
4. 任何使用GET方法的API将不会改变除日志之外的用户信息.
5. 任何使用POST方法的API将会改变用户的状态，信息或者数据.
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

7. 所有使用POST方法的API地址与GET必须一致。添加action方法指示当前操作的目的，如果create/update/delete/remove等。
    简单示例如下:
    > POST /user/1?action=update
    > POST /user?action=create  
    > POST /user/?action=remove  //删除全部
    > POST /user/1?action=remove  //删除1个

8. POST格式需要跟表单提交的格式相同。禁止提交json/xml文件。
    > POST /user?action=create
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

11. 错误以[errorable](https://github.com/calidion/errorable)方式定义，由[errorable-common](https://github.com/Errorable/common)库实现.
12. 参数保留字
* action:  表示操作动作
* page: 表示当前页
* limit: 表示每个分页大小
* token: 表示服务器的token
 后续还会不断的增加
