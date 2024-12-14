### Mongodb

1. 安装

   https://docs.mongodb.com/manual/tutorial/install-mongodb-on-red-hat/
   
2. mongodb 的常用命令：

   1. show dbs;    展示所有的数据库；
   2. db  ; 展示当前所使用的数据库；
   3. use db_name ;如果指定的数据库不存在，则创建新的数据库，否则切换到指定的数据库
   4. db.dropDatabase() ；删除当前所在的数据库；
   5. db.createCollection("table1"); 在当前数据中创建 集合table1；在Mongodb中不需要手动的创建集合，插入数据时， 会自动创建一个与db_name同名的集合。 
   6. db.collection_name.drop();  删除名称为collection_name的集合；
   7. show tables；   展示当前集合中的表的个数；
   8. show collections ; 展示当前数据库中有哪些集合；
   
3. mongodb的CRUD

   1. insert 插入

      1. db.connection_name.insert({dict});
      2. db.connection_name.save({});  

   2. 更新；

      1. db.connection_name.save({"_id":""}) 如果save中有id则为更新；没有Id测为插入；

      2. db.connection_name.update({},{$set:{}}) 
   3. 删除
   	1. db.connection_name.remove({})  删除所有的数据；

      2. db.connection_name.remove({"name":"stephen"}); //删除name为stephen的数据；

      3. 

      4. 

      5. 

      6. 管道：将前一个表达式的结果， 做为别一个表达式的输入。  
      
         
      
         
      
         2682523
      
         
      
      
      
      
      
      