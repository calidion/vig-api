[[English Version](https://github.com/calidion/egg/blob/master/README.md)]
[[中文版](https://github.com/calidion/egg/blob/master/README.zh-CN.md)]

# Egg API: a simple api for nodejs
Egg API is a simple api rule for http based api design.   
Egg API is to reduce complexity Restful Api introduced.

RESTful API problems:  
1. Complexity. There are at least five methods you should be handling.  
2. None productive. We must decide every time if we should use PUT/POST, where we need an eat/paint.  
3. None accurate. HTTP Methods is limited and none extensible, so the method can not be accurate for apis.  
4. None descriptive. Restful API sometimes can be too simple to understand.  

Rules:

1. Egg API uses HTTP GET/POST as the primary methods.
2. GET mehtod for data retrieving from server.
3. POST method for data sending to server.
4. Any API using GET method will not change any data of the user.
5. Any API using POST method will change the state, info or data of the user.
6. All APIs using GET method will start with a noun, like user/admin.  
  * a simple noun indicates that you want to retrieve all data  
  
    > GET /user  
  * default page limit is 20, if you want to change this limit, specify a value to limit, limit is only valid here.  
  
    > GET /user?limit=50  
  * default page number is 1, if you want to change page number, specify a value to page.  
  
    > GET /user?page=5  
  * page & limit can be combined as get parameter 
  
    > GET /user?page=5&limit=50  
  * followed a number or id to get a single item, key limit will have no effect here, and be ignored.  
  
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

11. Error code can be defined by [errorable](https://github.com/calidion/errorable) and it's library [common](https://github.com/Errorable/common).

