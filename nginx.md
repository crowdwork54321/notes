## 1. nginx在location中直接返回(注意状态码必须要，  default_type text/html;也必须加)
> 返回文本：
```
location ~ ^/get_text {
    default_type text/html;
    return 200 'This is text!';  
}
```


>返回json
```
location ~ ^/get_json {
    default_type application/json;
    return 200 '{"status":"success","result":"nginx json"}';
}
````


>返回字符串并指定字符集：
```
location ~ ^/get_text {
    default_type text/html;

    add_header Content-Type 'text/html;charset=utf-8'
    return 200 'This is text!';  
}
```

****




## 2. nginx Location的配置规败
>location可以由前缀字符串或正则表达式定义,正则表达式使用“ ~* ”修饰符（用于不区分大小写的匹配）或“ ~ ”修饰符（用于区分大小写的匹配）指定。为了找到与给定请求匹配的位置,nginx首先检查使用前缀字符串（前缀位置）定义的位置。 并且，选择并记住具有最长匹配前缀的位置;
location 的语法如下：
Syntax:	location [ = | ~ | ~* | ^~ ] uri { ... }
location @name { ... } <br/>
可参考：https://www.jianshu.com/p/a16936455018


1. = 精确匹配
```
location  = / {
  # 精确匹配 / ，主机名后面不能带任何字符串
  [ configuration A ]
}

```

2. 前缀匹配
```
'/' 会匹配站点下的所有路径，当最长前缀和正则都无法满足的时， '/'会匹配； 
location  / {

}
```

```
匹配任何以 /documents/ 开头的地址，匹配符合以后，记住还要继续往下搜索， 只有后面的正则
表达式没有匹配到时，这一条才会采用这一条
location /documents/ {

}
```

```
 匹配任何以 /documents/Abc 开头的地址，匹配符合以后，停止搜索
location ~ /documents/Abc {

}
```

```
匹配任何以 /images/ 开头的地址，匹配符合以后，停止往下搜索正则，采用这一条。
location ^~ /images/ {

}
```


## 3. root 与alias的区别：
```
浏览器中输入/static
实际上去找/var/www/app/static/static 这个路径了,详情参考这里：
https://stackoverflow.com/questions/10631933/nginx-static-file-serving-confusion-with-root-alias

location /static/ {
        root /var/www/app/static/;
        autoindex off;
}

```

### 4. nginx 重定向
```
server {
   listen 800;
   server_name olddomain.com;
   return 301 $scheme://newdomain.com$request_uri;
}
```
重定向所有olddomain.com 到newdomain.com 其中$scheme为原来请求协议（https/http)  ,$request_uri为原来的请求uri

```
rewrite ^/images/(.*)$ http://docs.domain2.com/$1 permanent;
rewrite ^/images/(.*)$ http://docs.domain2.com/$1 redirect;
```
issue a temporary redirection of the requests that come for the images folder to the new domain or the subdomain we specified.
更多信息请看：
https://www.nginx.com/blog/creating-nginx-rewrite-rules/


### 5.nginx配置请求头 add_header  'key'   'value'
```
    add_header X-Frame-Options 'SAMEORIGIN' always;
    add_header X-XSS-Protection '1; mode=block' always;
    add_header X-Content-Type-Options 'nosniff' always;
    add_header 'Access-Control-Allow-Origin' '$http_origin' always;
    add_header 'Access-Control-Allow-Credentials' 'true' always;
    add_header 'Access-Control-Max-Age' 86400 always;
    add_header 'Access-Control-Allow-Methods' 'GET, POST, PUT, PATCH, DELETE, OPTIONS' always;
    add_header 'Access-Control-Allow-Headers' 'DNT,X-Mx-ReqToken,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Authorization' always;
``` 

### 6.nginx 预定义的变量
```
$scheme  #为请求协议  http 或者https; 
$uri     #为请求uri,不带参数；
$request_uri   #带参数的uri;
$host    #为服务器主机；
$request_method  #为请求方法   GET POST; 
$query_string   #为请求字符串   name=stephen&age=30
$request_filename   #请求的文件与root和alias组合而成， 如/usr/share/ngin/html/img/cc
$document_root      #站点根目录     为server或Location中配置的目录；
```


### 7.nginx try_files的配置(try_files 可以用在Server和Location中)
```
try_files $uri $uri/ /index.html;   
第一步检查root/$uri 是否存在；
第二步检查root/$uri/默认首页是否存在；
第三步检查root/index.html 是否存在；
```

