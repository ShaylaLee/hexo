---
title: docker-常用操作
date: 2020-02-22 14:26:26
categories: docker
tags: 
- docker
- cmd
---

# docker

* 查看

```
docker info //Display system-wide information
```



* 登陆

```
docker login //登陆docker hub ID, 用于push和pull自定义镜像	
```



# 运行

- 拉取镜像

```shell
docker pull elasticsearch:6.7.0
```



- 运行容器

```shell
docker run 

-e ES_JAVA_OPTS="-Xms256m -Xmx256m"  //环境变量

-d  //守护进程运行

-p 9200:9200 -p 9300:9300  //端口映射

-v /data/mnt/elasticsearch/config/master.yml:/usr/share/elasticsearch/config/elasticsearch.yml //将host机器的目录mount到container中。

-v /data/mnt/elasticsearch/master:/usr/share/elasticsearch/data //https://www.jianshu.com/p/ef0f24fd0674

--name es-master  //容器命名，不允许重名。

elasticsearch:6.7.0 //镜像
```

 ```shell
docker run 

--link es-master:elasticsearch 

-p 5601:5601

--name kibana 

-d 

kibana:6.7.0
 ```



- 查看运行中的容器

```
docker ps 

docker container ls
```



- 查看所有容器，包括启动失败、停止的。

```
docker ps -a
```



- 查看某个容器的日志。

```
docker logs [--tail 10] {id/name}
```



- 启动容器

```
docker start {id/name}
```

-i:以 交互模式启动 [交互模式不懂点我](https://blog.csdn.net/michel4liu/article/details/80857995). (docker start -i {id/name}, 本会话内运行容器，关闭会话会停止容器)

-t:以 附加进程方式启动 [附加进程不懂的点我](https://blog.csdn.net/michel4liu/article/details/80878686)

 

- 暂停容器、恢复容器

```
docker pause {id/name}

docker unpause {id/name}
```



- 删除容器

```
docker container rm {id/name}
docker container rm $(docker container ls -a -q)  //删除所有容器
```



- 以命令行交互的方式 进入某个在运行的容器

```
docker exec -it {container-id/name} bash

docker exec -it {container-id/name} sh
```

不建议使用docker attach （关闭会话会停止容器） 

 

- 退出容器：exit

 

- 获取容器的PID

```
docker inspect --format "{{.State.Pid}}" {container-id/name}
docker top {container-id/name} //查看容器进程	
```



# 镜像

* 查看镜像

  ```
  docker images
  docker image ls -a
  docker image inspect app:v1
  docker images -f dangling=false // -f,--filter筛选
  ```

  

* 构建镜像

  在Dockerfile所在目录下执行

  ```
  docker build 
  --network host   //使用本机网络
  -t app:v1        //生成的镜像app,版本为v1
  .                //build上下文为当前目录
  ```

  

* 推送自定义镜像到远程仓库

  ```
  docker push <username>/<repository>:<tag>
  ```

  

* 删除镜像

  ```
  docker image rm app:v1
  docker rmi app:v1
  docker image prune //删除所有悬空镜像dangling images
  ```



* 新增tag

  ```
  docker image tag app:v1 app:v2
  ```



#  环境变量

* 设置环境变量：

1）通过dockerfile里的ENV命令设置 （跟随镜像）

2）通过`docker run --env <key>=<value>` 时指定或修改启动后的环境变量 （当前运行的容器生效）

* 查看环境变量：

```
docker inspect
docker exec -it <CONTAINER-NAME> OR <CONTAINER-ID> env
```



# 进入STOP的容器

找到想要进入的容器id, 假设是 837ffa1d4

保存"案发现场"为镜像

```
docker commit 837ffa1d4 user/temp
docker run -it user/temp sh //启动进入新容器
```





 

