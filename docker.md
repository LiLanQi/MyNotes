# docker常用命令

## **帮助命令**

```
docker version
docker info
docker 命令 --help
```



## **镜像命令**

```
docker images查看本地主机上的镜像
docker search 查找镜像
docker search 镜像 -f （条件）  过滤查找镜像
docker pull 下载镜像 

docker rmi 删除镜像（可接镜像image id）
```



## **容器命令**

说明：我们有了镜像才可以创建容器，下载一个centos镜像来测试学习

```
docker pull centos
```



```
docker run [可选参数] image    （docker run -it centos /bin/bash）


#### 参数说明

--name="Name" 容器名称 tomcat01 tomcat02，用来区分容器
-d                        后台方式运行
-it                        使用交互方式运行，进入容器查看内容
-p                        制定容器的端口 -p 8080:8080
	-p ip:主机端口:容器端口
	-p 主机端口:容器端口（常用）
	-p 容器端口
```

![image-20220129013534966](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220129013534966.png)

![image-20220129013638119](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220129013638119.png)

```
docker ps 列出正在运行的镜像（-a列出曾经运行过的镜像）
```

```
docker start 容器id，启动一个已经停止的容器
```

docker run -it -p 12345:8888 -v/home/hrren/llq:/usr/local/llq931 --gpus all  llq931 /bin/bash

### 退出容器

```
exit 直接容器停止并退出
Ctrl+P+Q 容器不停止退出
```



### 删除容器

```
docker rm 容器id    删除指定容器，不能删除正在运行的容器，除非使用rm -f
docker rm -f $(docker ps -aq) 删除所有容器
启动和停止容器的操作 
docker start 容器id，启动容器
docker restart 容器id，重启容器
docker stop 容器id，停止当前正在运行的容器
docker kill 容器id，强制停止当前容器
```



## 常用其他命令

### 后台启动容器

```
命令 docker run -d 镜像（docker run -d centos）

常见的坑，docker容器使用后台运行，就必须要有一个前台进程，docker发现没有应用，就会直接停止


```

### 查看日志

```
--tail number 显示日志条数
docker logs -tf --tail 10 容器 (dce7b86171bf)
```

### 查看容器中进程信息

```
docker top 容器id
(llq) hrren@wmgroup-PowerEdge-T630 ~/llq $ docker top 5348e807f29d
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
root                3909                3877                0                   02:21               pts/0               00:00:00            /bin/bash

```

### 查看镜像的元数据

```
docker inspect 容器id
docker inspect hello-world
[
    {
        "Id": "sha256:fce289e99eb9bca977dae136fbe2a82b6b7d4c372474c9235adc1741675f587e",
        "RepoTags": [
            "hello-world:latest"
        ],
        "RepoDigests": [
            "hello-world@sha256:0e11c388b664df8a27a901dce21eb89f11d8292f7fca1b3e3c4321bf7897bffe"
        ],
        "Parent": "",
        "Comment": "",
        "Created": "2019-01-01T01:29:27.650294696Z",
        "Container": "8e2caa5a514bb6d8b4f2a2553e9067498d261a0fd83a96aeaaf303943dff6ff9",
        "ContainerConfig": {
            "Hostname": "8e2caa5a514b",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/sh",
                "-c",
                "#(nop) ",
                "CMD [\"/hello\"]"
            ],
            "ArgsEscaped": true,
            "Image": "sha256:a6d1aaad8ca65655449a26146699fe9d61240071f6992975be7e720f1cd42440",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {}
        },
        "DockerVersion": "18.06.1-ce",
        "Author": "",
        "Config": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/hello"
            ],
            "ArgsEscaped": true,
            "Image": "sha256:a6d1aaad8ca65655449a26146699fe9d61240071f6992975be7e720f1cd42440",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": null
        },
        "Architecture": "amd64",
        "Os": "linux",
        "Size": 1840,
        "VirtualSize": 1840,
        "GraphDriver": {
            "Data": {
                "MergedDir": "/var/lib/docker/overlay2/9931503648f6eedfddc5c27d3e879933eeb33c99917ae9403c51c217529f3f2e/merged",
                "UpperDir": "/var/lib/docker/overlay2/9931503648f6eedfddc5c27d3e879933eeb33c99917ae9403c51c217529f3f2e/diff",
                "WorkDir": "/var/lib/docker/overlay2/9931503648f6eedfddc5c27d3e879933eeb33c99917ae9403c51c217529f3f2e/work"
            },
            "Name": "overlay2"
        },
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:af0b15c8625bb1938f1d7b17081031f649fd14e6b233688eea3c5483994a66a3"
            ]
        },
        "Metadata": {
            "LastTagTime": "0001-01-01T00:00:00Z"
        }
    }
]

```

### 进入当前正在运行的容器

```
我们通常容器都是使用后台方式运行的，需要进入容器，修改一些配置
方式一：
docker exec -it 容器id bashShell (/bin/bash)

(llq) hrren@wmgroup-PowerEdge-T630 ~ $ docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED          STATUS          PORTS     NAMES
1ddc68f5d573   centos    "/bin/bash"   16 minutes ago   Up 16 minutes             keen_hertz
(llq) hrren@wmgroup-PowerEdge-T630 ~ $ docker exec -it 1ddc68f5d573 /bin/bash
[root@1ddc68f5d573 /]#

方式二：
docker attach 容器id

前者进入容器后开启一个新的终端，可以在里面进行操作（常用）
后者进入容器正在执行的终端，不会启动新的进程

```

### 从容器内拷贝文件到主机上

```
(llq) hrren@wmgroup-PowerEdge-T630 ~/llq $ docker run -it centos /bin/bash
[root@5348e807f29d /]# cd /home
[root@5348e807f29d home]# touch test3.py
[root@5348e807f29d home]# (llq) hrren@wmgroup-PowerEdge-T630 ~/llq $ docker attach 5348e807f29d 用这个指令可以进入之前的终端，不然用第一种进入运行容器的方式只会开启一个新的终端，而不是之前创建test3.Py的终端
[root@5348e807f29d home]# cd /home
[root@5348e807f29d home]# ls
test3.py
[root@5348e807f29d home]# read escape sequence
(llq) hrren@wmgroup-PowerEdge-T630 ~/llq $ docker cp 5348e807f29d:/home/test3.py ./

拷贝是一个手动过程，未来我们使用-v卷的技术，可以实现，自动同步 /home  /home
```



### 实践项目

```
实践项目：部署nginx

1.  查找nginx     docker search nginx
2. 下载nginx      docker pull nginx
3. 运行nginx       docker run -d --name nginx0 -p 3344:80 nginx（以后台方式运行，命名为nginx0 ，开放容器内部端口80，外部端口3344，可以通过localhost:3344进行nginx访问）
```

```
实践项目：部署tomcat

1.  下载tomcat （docker run -it --rm tomcat:9.0）官方是这样使用的

   #我们之前的启动都是后台，停止了容器之后，容器还是可以查到的，docker run -it --rm一般用来测试，用完即删除

2. 正常下载 docker pull tomcat 

3. 运行tomcat docker run -d --name tomcat0 -p 3355:8080 tomcat

4. 发现不能正常访问，webapps里面没有东西，因为阿里云镜像的原因，默认是最小的镜像，所有不必要的都剔除掉
```

```
实践项目：部署es+kibana

1. 下载启动es    docker run -d --name elasticsearch  -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:7.6.2，非常非常耗内存，并且服务器很卡

 2. docker stats 查看状态

 3. 赶紧关闭，增加内存的限制，修改配置文件 -e 环境配置修改

    docker run -d --name elasticsearch  -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node"  -e ES_JAVA_OPTS="-Xms64m -Xmx512m" elasticsearch:7.6.2


```



### commit镜像

```
docker commit 提交容器成为一个新的副本

docker commit -m=“提交的描述信息” -a="作者" 容器id 目标镜像名：[TAG]

#如果你想要保存当前容器的状态，可以通过commit命令提交，获得一个镜像
```



# docker镜像讲解

## 容器数据卷

### 使用数据卷 

方式一：直接使用命令来挂载 -v

docker run -it -v 主机目录:容器内目录

(llq) hrren@wmgroup-PowerEdge-T630 ~/llq $ docker run -it -v /home/hrren/llq/dockerTest/test:/home centos
[root@1d49c7b19b4f /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var

通过docker inspect 1d49c7b19b4f，在里面可以看到下一行数据

"Mounts": [
            {
                "Type": "bind",
                "Source": "/home/hrren/llq/dockerTest/test",
                "Destination": "/home",
                "Mode": "",
                "RW": true,
                "Propagation": "rprivate"
            }
        ],



### 具名和匿名挂载

 

`#如何确定是具名挂载还是匿名挂载，还是指定路径挂载`

`-v 容器内路径 #匿名挂载`

`-v 卷名：容器内路径 #具名挂载`

`-v /宿主机路径：容器内路径 #指定路径挂载`

所有docker容器内的卷，没有指定目录的情况下都是在 `/var/lib/docker/volunes/xxxx/_data`



### 数据卷容器

通过--volume-from可以实现俩个容器数据同步

## DockerFile

### DockerFile介绍

dockerfile是用来构建docker镜像的文件!命令参数脚本!

构建步骤：

1. 编写一个dockerfile文件
2. docker build构建成为一个镜像
3. docker run运行镜像
4. docker push 发布镜像（DockerHub、阿里云镜像仓库）

### DockerFile构建过程

**基础知识**：

1. 每个保留关键字（指令）都是必须是大写字母
2. 执行从上到下顺序执行
3. #表示注释
4. 每一个指令都会创建提交一个新的镜像层，并提交

![image-20220423125934582](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220423125934582.png)

dockerfile是面向开发的，我们以后要发布项目，做镜像，就需要编写dockerfile文件，这个文件十分简单！

步骤：开发、部署、运维。。。缺一不可

Docker镜像逐渐成为企业交付的标准，必须要掌握！

DockerFile：构建文件，定义了一切的步骤，源代码

DockerImages：通过DockerFile构建生成的镜像，最终发布和运行的产品！

Docker容器：容器就是镜像运行起来提供服务的

### DockerFile的指令

```shell
FROM		# 基础镜像，一切从这里开始
MAINTAINER	# 镜像是谁写的，姓名+邮箱
RUN			# 镜像构建的时候需要运行的命令
ADD 		# 步骤：tomcat镜像，这个tomcat压缩包就是添加内容（自动解压）
WORKDIR		# 镜像的工作目录
VOLUME		# 挂载的目录
EXPOSE 		# 保留端口配置
CMD			# 指定这个容器启动的时候要运行的命令，只有最后一个会生效，可被替代
ENTEYPOINT	# 指定这个容器启动的时候要运行的命令，可以追加命令
ONBUILD		# 当构建一个被继承 DockerFile，这个时候就会运行 ONBUILD 指令，触发指令
COPY		# 类似ADD，将我们文件拷贝到镜像中
ENV			# 构建的时候设置环境变量
```

### 实战测试

Docker Hub中99%的镜像都是从这个基础镜像过来的 FROM scratch，然后配置需要的软件和配置来进行的构建

![image-20220423161507502](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220423161507502.png)

创建一个自己的centos

```shell
# 1 编写DockerFile文件
FROM centos
MAINTAINER llq<1042520531@qq.com>

ENV MYPATH /usr/loacl
WORKDIR $MYPATH

RUN yum -y install vim
RUN yum -y install net-tools

EXPOSE 80

CMD echo $MYPATH
CMD echo "-----end"
CMD /bin/bash

# 2 通过这个文件构建镜像
# 命令 docker build -f dockerfile文件路径 -t 镜像名:[tag]
docker build -f dockerfile-centos -t mycentos:0.1 .

# 3 测试镜像

```

CMD和ENTEYPOINT区别

```shell
FROM centos
CMD ["ls", "a"]
```

```shell
FROM centos
ENTEYPOINT["ls", "a"]
```

执行 docker run -it xxx -l，使用CMD构建的镜像无法执行（用-l替换了 ls -a导致报错)，使用ENTEYPOINT构建的镜像执行ls -al

### 实战：tomcat镜像

1. 准备镜像文件    tomcat压缩包，jdk压缩包

2. 编写dockerfile文件，官方命名 `Dockefile`， build会自动寻找这个文件，就不需要 -f 指定了！

   ![image-20220423173212220](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220423173212220.png)

3. 构建镜像，运行，测试

### 发布自己的镜像

1. dockerhub注册账号(阿里云)
2. docker login -u 账号名
3. docker push 用户名/镜像名字[tag：版本号]

## Docker网络



安装torch-geometric过程

 pip install git+https://github.com/pyg-team/pytorch_geometric.git

```
cfg0= benchmark: False											
bn:
  eps: 1e-05
  mom: 0.1
cfg_dest: config.yaml
custom_metrics: []
dataset:
  cache_load: False
  cache_save: False
  dir: ./datasets
  edge_dim: 128
  edge_encoder: False
  edge_encoder_bn: True
  edge_encoder_name: Bond
  edge_message_ratio: 0.8
  edge_negative_sampling_ratio: 1.0
  edge_train_mode: all
  encoder: True
  encoder_bn: True
  encoder_dim: 128
  encoder_name: db
  format: PyG
  label_column: none
  label_table: none
  location: local
  name: Cora
  node_encoder: False
  node_encoder_bn: True
  node_encoder_name: Atom
  remove_feature: False
  resample_disjoint: False
  resample_negative: False
  shuffle_split: True
  split: [0.8, 0.1, 0.1]
  split_mode: random
  task: node
  task_type: classification
  to_undirected: False
  transductive: True
  transform: none
  tu_simple: True
device: auto
gnn:
  act: relu
  agg: add
  att_final_linear: False
  att_final_linear_bn: False
  att_heads: 1
  batchnorm: True
  clear_feature: True
  dim_inner: 16
  dropout: 0.0
  head: default
  keep_edge: 0.5
  l2norm: True
  layer_type: generalconv
  layers_mp: 2
  layers_post_mp: 0
  layers_pre_mp: 0
  msg_direction: single
  normalize_adj: False
  self_msg: concat
  skip_every: 1
  stage_type: stack
gpu_mem: False
mem:
  inplace: False
metric_agg: argmax
metric_best: auto
model:
  edge_decoding: dot
  graph_pooling: add
  loss_fun: cross_entropy
  match_upper: True
  size_average: mean
  thresh: 0.5
  type: gnn
num_threads: 6
num_workers: 0
optim:
  base_lr: 0.01
  lr_decay: 0.1
  max_epoch: 200
  momentum: 0.9
  optimizer: adam
  scheduler: cos
  steps: [30, 60, 90]
  weight_decay: 0.0005
out_dir: results
print: both
round: 4
seed: 0
share:
  dim_in: 1
  dim_out: 1
  num_splits: 1
tensorboard_agg: True
tensorboard_each_run: False
train:
  auto_resume: False
  batch_size: 16
  ckpt_clean: True
  ckpt_period: 100
  enable_ckpt: True
  epoch_resume: -1
  eval_period: 10
  iter_per_epoch: 32
  mode: standard
  neighbor_sizes: [20, 15, 10, 5]
  node_per_graph: 32
  radius: extend
  sample_node: False
  sampler: full_batch
  skip_train_eval: False
  walk_length: 4
val:
  node_per_graph: 32
  radius: extend
  sample_node: False
  sampler: full_batch
view_emb: False
```

