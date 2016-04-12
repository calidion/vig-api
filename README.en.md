[[中文版](https://github.com/calidion/egg/blob/master/README.md)]
[[English Version](https://github.com/calidion/egg/blob/master/README.en.md)]


# Egg API: A simple rule set for RESTful-like APIs 

Is is a simpler rule set for HTTP based APIs, it tries to reduce the complexity that the RESTful APIs introduced.

## Problems of RESTful APIs

1. Using too many HTTP methods, a lot of them are not popular among programmers.
2. Though a lot HTTP methods used, they are not adequate for a lot of cases.
3. Some methods like PUT, POST are confusing in their differences, and hard to decide which one to use.
4. The way the RESTful apis operate resources through HTTP methods are vague and hard to understand, it sucks a lot of programmers, makes it hard to be realized.
5. Too academic, not practical.

## Why Egg API

1. To lower the complexity, to simplify the way an API defined..
2. More practical, more operational, more intuitive.
3. Keep the good parts from RESTful APIs
4. Drop the bad or inpratical parts from RESTful APIs
5. More actions customizable
6. Good parts such as stateless, resouce locatable, etc. are kept.


## Intended Audiance

1. Small companies with angile teams
2. Big companies with newbies.


## Rules

1. Use GET/POST methods as the only primary methods.
2. The GET mehtod is for data retrieving only.
3. The POST method is for data sending only.
4. APIs using GET method will not change any data of the resources.
5. APIs using POST method will change the data of the resources.
6. All APIs using GET method will start with a noun, like user/1.  
  * a simple noun indicates that you want to retrieve all data  
  
    > GET /user  
  * Page limit defaults to 20, specify a value to change the limit.  
  
    > GET /user?limit=50  
  * Page number defaults to 1, specify a value to change the page number.  
  
    > GET /user?page=5  

  * Appending a number or an id or a string to the noun indicates a more specific entity of the noun. And this way will invalidate the parameters limit and page
  
    > GET /user/1  
    > GET /user/1?limit=50&page=1    //the Parameters limit and page are useless

7. URIs are identical to the same resources regardless of the HTTP methods(GET or POST). We need to add the <code>action</code> parameter to indicate the operation on the resouces, such as actions: create/update/delete/remove
    Simple examples:
    > POST /user/1?action=update  
    > POST /user?action=create    
    > POST /user/?action=remove  //Delete all 
    > POST /user/1?action=remove  //Delete one  

8. the POST request should be the same as a web form POST request. Should never post json/xml files.  
     > POST /user?action=create
     > ...
     > 
     > 
     > name=aaa&password=asdfsf
9. All APIs return json only.
10. The json returned should at lease have the following fields:

    | field name | Description |
    | --- | --- |
    | code | error code|
    | name | error name|
    | message | error message|
    | data | return data |

11. Errors implementation   
    Refer to [errorable](https://github.com/calidion/errorable) and it's error realization [common](https://github.com/Errorable/common).
13. Reserved parameters:
  * action: actions to be operated on resouces
  * page: page number of resouces
  * limit: page limit of resouces
  * token: token for the client

