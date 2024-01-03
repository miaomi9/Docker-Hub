# Docker Hub 镜像加速器

国内从 Docker Hub 拉取镜像有时会遇到困难，此时可以配置镜像加速器。Docker 官方和国内很多云服务商都提供了国内加速器服务。

> Dockerized 实践 https://github.com/y0ngb1n/dockerized

## 配置加速地址

> Ubuntu 16.04+、Debian 8+、CentOS 7+

创建或修改 `/etc/docker/daemon.json`：

```bash
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
    "registry-mirrors": [
        "https://dockerproxy.com",
        "https://docker.mirrors.ustc.edu.cn",
        "https://docker.nju.edu.cn"
    ]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

对于 Mac 和 Windows 用户，直接在 Docker Desktop 系统设置中，配置 registry-mirrors 即可。

---
## 检查加速器是否生效

命令行执行 `docker info`，如果从结果中看到了如下内容，说明配置成功。

```console
Registry Mirrors:
 [...]
 https://docker.m.daocloud.io
```

## Docker Hub 镜像测速

使用镜像前后，可使用 `time` 统计所花费的总时间。测速前先移除本地的镜像！ 

```console
$ docker rmi node:latest
$ time docker pull node:latest
Pulling repository node
[...]

real   1m14.078s
user   0m0.176s
sys    0m0.120s
```
---
## Docker Hub 镜像加速器列表
以下镜像站来源于互联网（感谢热心网友），可能出现宕机、转内网、关停等情况，建议同时配置多个镜像源。

镜像加速器 | 镜像加速器地址 | 说明| 其它加速
--- | --- | --- | ---
[DaoCloud 镜像站](https://github.com/DaoCloud/public-image-mirror) | `https://docker.m.daocloud.io` | |  Docker Hub、GCR、K8S、GHCR、Quay、NVCR 等
[网易云](https://c.163yun.com/hub) | `https://hub-mirror.c.163.com` | | Docker Hub
[Docker 镜像代理](https://dockerproxy.com) | `https://dockerproxy.com` | | Docker Hub、GCR、K8S、GHCR
[百度云](https://cloud.baidu.com/doc/CCE/s/Yjxppt74z#%E4%BD%BF%E7%94%A8dockerhub%E5%8A%A0%E9%80%9F%E5%99%A8) | `https://mirror.baidubce.com` | | Docker Hub
[南京大学镜像站](https://doc.nju.edu.cn/books/35f4a) | `https://docker.nju.edu.cn` | | Docker Hub、GCR、GHCR、Quay、NVCR 等
[上海交大镜像站](https://mirrors.sjtug.sjtu.edu.cn/) | `https://docker.mirrors.sjtug.sjtu.edu.cn` | | Docker Hub、GCR 等
[中科院软件所镜像站](https://mirror.iscas.ac.cn/mirror/docker.html) | `https://mirror.iscas.ac.cn` | | Docker Hub

## 参考链接

+ https://docs.docker.com/registry/recipes/mirror/
+ https://github.com/yeasy/docker_practice/blob/master/install/mirror.md
+ https://www.ilanni.com/?p=14534
+ https://moelove.info/2020/09/20/突破-DockerHub-限制全镜像加速服务/
+ https://gist.github.com/y0ngb1n/7e8f16af3242c7815e7ca2f0833d3ea6
