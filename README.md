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
6. 所有使用GET方法的API必须以英文名词开头，并要求采用复数形式。比如users/1， users/1。

  * 只写一个名词表示你需要获取到所有的信息  
    
    > GET /users
  * 默认分页的大小是20个数据，如果你想修改这个默认值，可以添加limit的值，格式如下:  
      
    > GET /users?limit=50
  * 默认页面是第一页，如果你想修改当前的页面数，可以指定page值，格式如下:  
      
    > GET /users?page=3
  * 在名词后接一个数值或者一个ID可以获得一个数据项，这时limit与page无效，会被忽略。  
      
     > GET /users/1  
     > GET /users/1?limit=50&page=10  

7. 所有使用POST方法的API地址与GET必须一致。
8. 在POST中使用action字段表示方法。
   * 也可以将action作为POST的数据的一部分提交到服务器，如    
    > POST /users  
    > ...    
    >    
    >     
    > <code>action=create</code>&name=aaa&password=asdfsf  


9.  禁止提交json/xml等文件。

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

13. 参数保留字 （主要用于query）
  * action:  表示操作动作,限用于POST  
  * page: 表示当前页  
  * limit: 表示每个分页大小  
  * token: 表示服务器的token   
  * state: 表示资料的状态
  * from: 表示开始时间
  * to: 表示结束时间
  
## URI的query参数规范

在蛋蛋API里完全将Query当成是查询，但是除去了ID查询。
因为通过URI就可以定位ID查询的结果。

在URI中第一个?号后的参数称为query参数，一般的形式是name=value&name1=value1这样的。
在这里，我们基于HTTP的query，实现对数据的查询。

### 分页

参数名page表示页码，limit表示每页数据量。

所以获取第5页，每页50个数据的query是这样的：

```
uri?page=5&limit=50
```
默认值： page=1, limit=20。

### 会话

如果蛋蛋API的会话是基于token的。token将会被放在query里。

代码示例：
```
uri?token=xxx
```

### 状态查询

每个业务资源都是可以有状态的，所以我们提供了state来表示状态。
建议所有的状态都使用state来表达。同时状态值使用大写字符串来表达。

代码示例：
```
uri?state=GOOD
```

### 时间段匹配

在query里面提供了时间查询字串：from, to。
可以单独使用，也可以混合使用。查询格式是 YYYY-MM-DD HH:MM:SS。
可以不断减少精确度，直到只有年。
所以可以是 
YYYY-MM-DD HH:MM,  YYYY-MM-DD HH,  YYYY-MM-DD,  YYYY-MM,  YYYY
这几种格式。
月日不足10时需要使用0补足。
小时采用24小时制。

代码示例：
```
uri?from=1998-09-01&to=2000-01-20 20:10
```

## URI的POST参数规范

由于蛋蛋API采用POST来改变数据，所以我们对POST数据作出如下规范：

1. 提交的数据要与HTTP的表单提交一致  
2. 以action字段取代RESTful APIs里面的HTTP方法  
3. 所以业务变更必须通过类HTTP的表单提交  
4. 不能将业务逻辑写在文件里，通过文件提交。比如将业务逻辑写到JSON文件里提交到服务器  
5. 所有的变更参数必须通过POST提交  

一个POST示例：

```

POST   /users


action=register&name=aaa&password=asdfsf
```




