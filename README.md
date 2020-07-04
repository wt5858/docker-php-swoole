# docker-php-swoole
Dockerfiles supporting PHP working environment

## 简介
用 Docker 容器服务的方式搭建 PHP 环境，易于维护、升级。使用前需了解 Docker 的基本概念，常用基本命令。
docker-compose可以一条docker命令来构建镜像，容器。

相关软件版本：
- PHP 7.3
- MySQL 5.7
- Nginx 1.17.3
- Tengine: 2.3.2
- Redis 5.08

## 使用
### 1.安装 Docker，Docker-compose  
- Docker，详见官方文档：https://docs.docker.com/engine/installation/linux/docker-ce/centos/
- docker-compose，文档：https://docs.docker.com/compose/install/
```
sudo pip install -U docker-compose
```

### 2.下载 docker-php-swoole
直接 clone：
```
git clone git@github.com:wt5858/docker-php-swoole.git
```
或者下载 zip 压缩包也可以。

### 3.docker-compose 构建项目
进入 docker-compose.yml 所在目录（也就是file目录）：
- 执行命令：
```
docker-compose up
```  

- 没问题，就让容器后台运行：  
```
docker-compose up -d
``` 

- 其它容器操作
```
docker-compose start xxx  启动某个容器

docker-compose stop  xxx  停止某个容器

docker-compose restart xxx 重启某个容器

docker-compose logs xxx 查看某个容器的日志

docker-compose exec xxx bash  以命令行的方式进入容器（有的可能是把bash换成sh） 
``` 

- 启动成功后可以看下容器是否运行正常
```
docker ps
```
- 如果有没启动成功的就可以先查看日志，修改后再重新启动

- 这里可能会出现的问题有 data里面的目录权限问题，可以如下修改
```
chmod -R 755 xxx
或
chown -R www-data:www-data xxx
```


- 关闭所有容器并删除服务：
```
docker-compose down
```

### 4. 使用 Composer

在创建 PHP-fpm 容器时就已经将 Composer 安装在容器中，可以运行该容器进行 Composer 操作。

用 docker-compose 进行操作：
```
docker-compose run --rm -w /data/www/xxx php-fpm composer update
```
`-w /data/www/xxx`中的`xxx`为在项目名（对应app目录里面的项目名），项目是挂载在php-fpm里面的，我们可以直接在容器里运行composer。

或者进入容器 再用 docker 命令：
```
docker-compose exec php-fpm bash

cd /data/www/xxx

composer update
```

### 5. 使用 supervisor

以命令行方式进入php-fpm容器
```
docker-compose exec php-fpm bash
```
启动supervisor服务（如果之前已经启动了就不需要了）
```
/usr/bin/supervisord  -c /etc/supervisord.conf  
```
更新supervisor配置
```
supervisorctl update
```
其它supervisor操作
```
supervisorctl status 
supervisorctl start xxx 
supervisorctl stop xxx 
```
