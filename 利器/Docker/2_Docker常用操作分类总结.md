#常用操作分类总结
【归纳整理Docker基本知识/Docker入门操作、Docker基本命令】


###镜像操作
检索 docker search 关键字  
    比如docker search mysql，会去docker仓库docker hub中搜索与mysql相关的镜像  
    
拉取 docker pull 镜像名[:tag]  
    比如docker pull mysql，就会去下载镜像到本地  
    :tag名是用来区分版本的，不写的话默认latest版本  
    
列表 docker images  
    查看本地所有镜像  
    
删除 docker rmi 镜像ID
    ·以下存疑
    docker rmi 用户名/仓库名:标签	【空格隔开可删除多个】
    docker rmi 'docker images -a -q'
    ·
    
运行 docker run --name 自定义名 -d 镜像名:标签名
    --name 自定义名 是为容器起一个名字
    -d 是指在后台运行
    -e 后面可以加运行参数
    -p 端口映射，如 -p 8080:8080，即主机8080端口映射为容器8080端口
    标签名为latest时可省略
创建
创建历史
推送

###容器操作
运行 docker run --name 自定义名 -d 镜像名:标签名
    --name 自定义名 是为容器起一个名字
    -d 是指在后台运行
    -e 后面可以加运行参数
    -p 端口映射，如 -p 8080:8080，即主机8080端口映射为容器8080端口
    标签名为latest时可省略
列表 docker ps
    查看有哪些容器在运行
    -a 列出所有容器，无论此时运行还是停止
    -l 列出最后一个运行的容器，无论此时运行还是停止
    -n N 列出最后的N个容器
详细信息
删除 docker rm 容器名或容器ID
停止 docker stop 容器名或容器ID
启动 docker start 容器名或容器ID
重启 docker restart 容器名或容器ID
自动重启
退出
守护
附着
日志 docker logs 容器名或容器ID
查看进程
端口 docker port 容器名或容器ID 容器端口号
    查看容器的端口映射情况




