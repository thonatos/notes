MagiciTerm Golang版本的请求结构接口。  

#### Structure

* Api (interface) 
    * post
    * user
* Request
    (/:model/:action/:parm)
    * post
        (文章：增，删，改，查)
        * add (对应的数据库中文章表的字段 数据类型)
            * title
            * tags 
            * type
            * date
            * time 
            * author
            * content
            * shortname
            * category
        * del
        * update
        * list
        * lists        
    * user     
        (用户：登陆，退出)    
        * login    
            request:
            * username
            * userpass
            * verified_code
            * verified_token
            return:
            * user_id
            * user_role
        * logout
            request:
            * null
            return:  
            * user_id
            * user_role 