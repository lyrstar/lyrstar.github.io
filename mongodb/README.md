- [回到首页](../README.md)

## 官网

[打开官网](https://www.mongodb.com/)


## Mongoose

[英文官网](https://mongoosejs.com)

[中文官网](https://mongoosejs.net)


## 已开启身份验证的情况下创建新库

```cmd
use admin

db.auth("adminUserName", "adminPassWord")

use newDBName

db.createUser({"user":"newDBUserName","pwd":"newDBPassWord",roles:["readWrite"]})

db.auth('newDBUserName', 'newDBPassWord')

db.test.insert({id:1})
```

***

## docker配置mongodb副本集

1. 启动三个mongodb进程

    ```cmd
    docker run -d -p 27017:27017 --name mongo mongo --replSet repset
    docker run -d -p 27018:27017 --name mongo1 mongo --replSet repset
    docker run -d -p 27019:27017 --name mongo2 mongo --replSet repset
    ```

2. 进入 mongodb docker

    ```cmd
    docker run -it  --name mongo-client mongo /bin/bash
    ```

3. 进入要作为master数据库的 mongodb shell
	
	```cmd
    mongo 192.168.0.1:27017/admin
    ```

4. 初始化副本集, _id和启动时设置的replSet参数相同
	
	```cmd
    rs.initiate({ _id:"repset", members:[
        {_id:0,host:"192.168.0.1:27017"}, {_id:1,host:"192.168.0.1:27018"}, {_id:2,host:"192.168.0.1:27019"}
    ]})
    ```
	
5. 查看副本集状态

	```cmd
    rs.status()
    ```
 
