[[中文版](https://github.com/calidion/egg/blob/master/README.md)]
[[English Version](https://github.com/calidion/egg/blob/master/README.en.md)]


# 蛋蛋 API: 一个简单的类RESTful API规范

蛋蛋API是一个更简单的基于HTTP的API规则。
蛋蛋API的目标是降低RESTful API的复杂性。

## RESTful API的问题

1. 使用过多的HTTP方法，很多方法大部分程序员并不熟悉
2. 虽然使用了很多的HTTP方法，但是这些方法跟现实比又显得不够用
3. PUT/POST等方法语义接近，区分困难
4. RESTful通过HTTP方法来操作资源的方式并不容易理解，导致大部分程序员不能真正的理解RESTful API的意义，从而无法实践
5. 过于学术化，实用性相对较差
6. 将网络底层协议与应用的API结合会造成协议规范与应用API规范的混乱
7. 无状态对于非用户的交互机制来说是方便的，但是对于用户交互来说显得有点繁琐。

## 为什么要定义蛋蛋API？

1. 降低门槛，简单化
2. 更加符合实际，好操作，好使用，更接近人的使用直觉
3. 吸引RESTful里精华的部分
4. 去掉RESTful里不实用的部分
5. 可以自定义更多的动作
6. 支持COOKIE、SESSION、TOKEN等多种形态
7. 不只是资源表达的API，也是业务表达的API
8. 不建议将资源放在API里。（比如下载个WORD文档或者图片什么的）
9. 所有的资源以静态文件的形式保存
10. API只负责提供业务资源，而不是文件资源，不需要更多的MIME支持。

## 适用对象
1. 小公司，敏捷团队
2. 大公司，新手团队

## 蛋蛋API规范

1. 蛋蛋API只将HTTP的GET/POST方法作为基本方式。
2. GET方法用于从服务器获取数据。
3. POST方法用于向服务器发送数据。
4. 任何使用GET方法的API将不会改变除日志之外的用户信息。
5. 任何使用POST方法的API将会改变用户的状态，信息或者数据。
6. 所有使用GET方法的API必须以英文名词开头，比如user/1。

  * 只写一个名词表示你需要获取到所有的信息  
    
    > GET /user
  * 默认分页的大小是20个数据，如果你想修改这个默认值，可以添加limit的值，格式如下:  
      
    > GET /user?limit=50
  * 默认页面是第一页，如果你想修改当前的页面数，可以指定page值，格式如下:  
      
    > GET /user?page=3
  * 在名词后接一个数值或者一个ID可以获得一个数据项，这时limit与page无效，会被忽略。  
      
     > GET /user/1  
     > GET /user/1?limit=50&page=10  

7. 所有使用POST方法的API地址与GET必须一致。
8. 在POST中使用action字段表示方法。
   * 也可以将action作为POST的数据的一部分提交到服务器，如    
    > POST /user  
    > ...    
    >    
    >     
    > <code>action=create</code>&name=aaa&password=asdfsf  


9. POST格式需要跟表单提交的格式相同。禁止提交json/xml文件。
    > POST /user?action=create  
    > ...  
    > 
    > 
    > name=aaa&password=asdfsf

10. 所有的API返回JSON数据，也就是将资源与API分开。

11. 所有的JSON数据包括以下字段：

    | 字段名 | 描述 | 备注 |
    | --- | --- | --- |
    | code | 错误代码 | 由Errorable Common包规范，当且仅当code为0时表示成功|
    | name | 错误名称|    |
    | message | 错误消息|    |
    | data | 返回数据 |    |

12. 错误实现  
    参考以[errorable](https://github.com/calidion/errorable)方式定义，[errorable-common](https://github.com/Errorable/common)库方式的实现。

13. 参数保留字
  * action:  表示操作动作,限用于POST  
  * page: 表示当前页  
  * limit: 表示每个分页大小  
  * token: 表示服务器的token    
 后续还会不断的增加
