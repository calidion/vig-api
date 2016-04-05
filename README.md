# Egg API
Egg API is a simple api rule for http based api design.   
Egg API is to reduce complexity Restful Api introduced.

1. Egg API uses HTTP GET/POST as the primary methods.
2. GET mehtod for Data retrieving from server.
3. POST method for Data sending to server.
4. Any API using GET method will not change any data of the user.
5. Any API using POST method will change the state, info or data of the user.
6. All APIs using GET method will start with a noun, like user/admin.  
  * simply a noun indicates that you want to retrieve all data  
    > GET /user  
  * default page limit is 20, if you want to change this limit, specify a value to limit, limit is only valid here.  
    > GET /user?limit=50  
  * default page number is 1, if you want to change page number, specify a value to page.  
    > GET /user?page=5  
  * page & limit can be combined as get parameter 
    > GET /user?page=5&limit=50  
  * followed a number or id to get a single item, limit will have no effect here, and be ignored.  
    > GET /user/1  
    > GET /user/1?limit=50 

7. All api using POST method must start with a verb, like create/update/remove which will change something, but not get/list/show which will not make can changes.
    a simple example will be:
    > POST /create/user  
    > POST /update/user  
    > POST /remove/user  

8. POST format should be in accordance with form post. Should never post a json/xml files as the parameter container.
     > POST /create/user
     > ...
     > 
     > 
     > name=aaa&password=asdfsf
9. All APIs return json only.
10. return json should at lease have the following fields:

    | field name | Description |
    | --- | --- |
    | code | error code|
    | message | error message|
    | data | return data |

11. Error code is primarily defined by [errorable](https://github.com/calidion/errorable) and it's library [common](https://github.com/Errorable/common).


# 蛋蛋 API
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


