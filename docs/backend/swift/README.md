---
 Swift
---

## 借助Vapor工具 使用Swift搭建api接口

```
以下是利用vapor框架，将swift写的api项目部署到后台云服务器中，总结的步骤，每台机器环境不一样，仅供参考。
```

## vapor and swift 


mysql | nginx | psql | swiftenv | 

```sh
$ sudo apt-get update 
$ sudo apt-get upgrade
$ eval "$(curl -sL https://apt.vapor.sh)"
$ sudo apt-get install swift vapor
$ sudo apt-get install nginx
$ sudo apt-get install supervisor
```

## 配置域名解析

A --- api.xxx.com 

## [nginx](https://docs.vapor.codes/2.0/deploy/nginx/)

```nginx

# /etc/nginx/site-avaliables/default
server {
    server_name hello.com;
    listen 80;
    root /home/vapor/Hello/Public/;
    try_files $uri @proxy;
    location @proxy {
        proxy_pass http://127.0.0.1:8080;
        proxy_pass_header Server;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_pass_header Server;
        proxy_connect_timeout 3s;
        proxy_read_timeout 10s;
    }
}
```
```
sudo service nginx stop
sudo service nginx start
sudo service nginx restart
```

 ``` 
【注意】如果nginx端口等相关配置更改了，需要执行以上3句命令 ，如果8080端口配置出错，可以改用其他端口，Swift项目中也相应修改
 ```

## [Supervisor](https://docs.vapor.codes/2.0/deploy/supervisor/)

/etc/supervisor/conf.d/


```

[program:hello]
command=/home/vapor/hello/.build/release/Run serve --env=production
directory=/home/vapor/hello/
user=www-data
stdout_logfile=/var/log/supervisor/%(program_name)-stdout.log
stderr_logfile=/var/log/supervisor/%(program_name)-stderr.log
```

```
supervisorctl reread
supervisorctl add hello
supervisorctl start hello
```

```
【注意】  如果执行$supervisorctl start hello后报错，“supervisor: child process was not spawned” ，
可以通过下面的命令来处理

备用命令
sudo supervisorctl stop all
sudo supervisorctl reread
sudo supervisorctl reload
sudo supervisorctl start all
sudo supervisorctl restart api (备注：刷新api配置，api替换成你自己的)


```

#### 接口修改上线流程

本案例通过git管理代码，在后端ubuntu系统项目路径拉取最新最新代码，

```
git pull
```

项目路径下，通过vapor编译成release包 

```
vapor build --release
```

如'/home/ubuntu/QsonAppApi/.build/release/Run'，后面会对Run进行访问，涉及权限问题，需要给Run设置权限，避免后续因权限问题报错

```
sudo chmod a+x /home/ubuntu/QsonAppApi/.build/release/Run
```

然后执行supervisorctl相关命令，步骤常驻后台设置


刷新
```
sudo supervisorctl restart api
```


其他说明：
`cat /etc/supervisor/conf.d/api.conf` 可以查看supervisor接口相关配置,也可把‘user=www-data’去掉


本帖引用自 [OHeroJ](https://github.com/OHeroJ) 的 原创帖 [Vapor3系列之hello小项目从0到部署上线](https://juejin.im/post/5d3196fcf265da1ba84acb31)