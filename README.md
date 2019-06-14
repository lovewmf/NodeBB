## NodeBB
![image](https://raw.githubusercontent.com/lovewmf/images/master/reade/des.png)
![image](https://raw.githubusercontent.com/lovewmf/images/master/reade/user.png)
![image](https://raw.githubusercontent.com/lovewmf/images/master/reade/admin.png)
NodeBB论坛由Node.js提供技术支持，并构建在Redis或MongoDB数据库上。支持响应式，它利用Web套接字进行即时交互和实时通知。NodeBB具有许多开箱即用的现代功能，如社交网络集成和流媒体讨论，同时仍确保与旧版浏览器兼容。

## 安装文档

[NodeBB Github](https://github.com/NodeBB/NodeBB)

* 安装MongoDB

    > 1. [MongoDB下载地址](https://www.mongodb.com/download-center/community)
    > 2. 安装直接下一步操作即可，需要注意"install mongoDB compass" 不勾选，否则可能要很长时间都一直在执行安装，MongoDB Compass 是一个图形界面管理工具
    > 3. 默认安装位置是 C:\Program Files\MongoDB\Server\4.0
    > 4. 将C:\Program Files\MongoDB\Server\4.0\bin添加到环境变量PATH中
    > 5. 为数据库和日志文件创建两个目录我们将使用C:\MongoDB\data\db和C:\MongoDB\logs
    > 6. 创建C:\MongoDB\mongod.cfg定义这些路径的配置文件
    
    > 7. 
    ```
    systemLog: destination: file path: C:\MongoDB\logs\mongod.log storage: dbPath: C:\MongoDB\data\db
    security:
    authorization: enabled
    ```
  > 8. 验证MongoDB的安装
  ```
  mongod --version
  ```
  > 9. 启动mongodb
  ```
  net start MongoDB 
  ```
  > 10. 设置MongoDB 数据库密码并创建nodebb数据库
  ```
  >> mongo
  >> use admin
  >> db.createUser( { user: "admin", pwd: "123456", roles: [ { role: "root", db: "admin" } ] } )
  >> use nodebb
  >> db.createUser( { user: "nodebb", pwd: "123456", roles: [ { role: "readWrite", db: "nodebb" }, { role: "clusterMonitor", db: "admin" } ] } )
  >> quit()
  ```
  > 11. 重启MongoDB并验证之前创建的管理用户是否可以连接
  ```
   net stop MongoDB
   net start MongoDB
   mongo -u admin -p 123456 --authenticationDatabase=admin
  ```
  
* 安装Nginx

    > 1. [Nginx下载地址](http://nginx.org/en/download.html)

    > 2. 在nginx\conf\nginx.conf添加一下配置

    > 3. 
    ```
    server {
        listen 80;
        server_name 127.0.0.1;
        location / {
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
            proxy_set_header Host $http_host;
            proxy_set_header X-NginX-Proxy true;
            proxy_pass http://127.0.0.1:4567;
            proxy_redirect off;
            # Socket.IO Support
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";
        }
    }
    ```
    > 4. 双击nginx.exe即可启动Nginx服务器


## 安装依赖并连接NodeBB

>> 默认根目录下没有package.json,不可以使用npm install 只需要执行一下命令

>> 安装依赖
```
nodebb setup 
```
>> 启动NodeBB
```
nodebb start
```
>> 前台首页为：http://127.0.0.1

>> 后台管理页面为：http://127.0.0.1/admin

## 注意事项
>> 如果控制台包403错误或者与NodeBB连接断开,在根目录下config.json里面添加如下配置
```
{
    "url": "http://192.168.1.112:4567",//请在cmd 执行ipconfig查看当前ip
    "secret": "225f65ee-74be-4196-bb8f-cb3be373d678",
    "database": "mongo",
    "port": "4567",
    "mongo": {
        "host": "127.0.0.1",
        "port": "27017",
        "username": "nodebb",
        "password": "123456", 
        "database": "nodebb",
        "uri": ""
    },
    "socket.io": {
        "origins": "*:*",
        "address": "",
        "transports": ["websocket", "polling"]
    }
}
```

>>[config.json配置文档](https://docs.nodebb.org/configuring/config/)

## NodeBB命令帮助

nodebb help



