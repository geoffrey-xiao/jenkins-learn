```bash
docker build -t myjenkins-blueocean:v1.0.0 .
```

export https_proxy=http://127.0.0.1:7890 http_proxy=http://127.0.0.1:7890 
all_proxy=socks5://127.0.0.1:7890

docker pull镜像时怎么使用代理

在使用`docker pull`拉取镜像时设置代理，你需要对Docker守护进程（dockerd）进行配置，因为它是负责拉取镜像的。这里有一些步骤可以帮助你通过代理服务器拉取Docker镜像：

1. **创建Docker服务目录**：首先，你需要创建一个特定的目录来存放Docker服务的配置文件。
```bash
sudo mkdir -p /etc/systemd/system/docker.service.d
```

2. **创建代理配置文件**：在上面创建的目录中，创建一个名为`http-proxy.conf`的文件。
```bash
sudo touch /etc/systemd/system/docker.service.d/http-proxy.conf
```

3. **编辑代理配置文件**：使用文本编辑器，如vim或nano，编辑`http-proxy.conf`文件，添加以下内容，将`proxy.example.com:8080`替换为你的代理服务器地址。
```bash
[Service]
Environment="HTTP_PROXY=http://proxy.example.com:8080"
Environment="HTTPS_PROXY=http://proxy.example.com:8080"
Environment="NO_PROXY=localhost,127.0.0.1,.example.com"
```

4. **重新加载系统配置**：修改配置文件后，你需要重新加载systemd的配置并重启Docker服务。
```bash
sudo systemctl daemon-reload
sudo systemctl restart docker
```

这样设置后，当你使用`docker pull`命令时，Docker守护进程会通过配置的代理服务器拉取镜像¹²。

请注意，如果你的代理服务器需要认证，你可能需要在代理URL中包含用户名和密码。此外，如果你在使用私有的Docker镜像仓库，你可能需要在`NO_PROXY`变量中添加它的地址，以确保Docker直接连接到这些地址，而不是通过代理服务器¹。

Source: Conversation with Copilot, 6/29/2024
(1) 如何优雅的给 Docker 配置网络代理 - CharyGao - 博客园. https://www.cnblogs.com/Chary/p/18096678.
(2) 如何配置docker通过代理服务器拉取镜像 - 醉马踏千秋 - 博客园. https://www.cnblogs.com/abc1069/p/17496240.html.
(3) docker设置代理解决内网pull外网镜像 - CSDN博客. https://blog.csdn.net/qq_43439214/article/details/135774453.
(4) docker pull 设置代理-CSDN博客. https://blog.csdn.net/weixin_34344403/article/details/91436859.
(5) docker pull 设置代理. https://www.bboy.app/2019/04/30/docker-pull-%E8%AE%BE%E7%BD%AE%E4%BB%A3%E7%90%86/.
(6) undefined. http://proxy.example.com:8080/.
(7) undefined. http://proxy.example.com:8080.
(8) undefined. https://registry-1.docker.io.
(9) undefined. http://proxy.example.com:80.
(10) undefined. https://proxy.example.com:443.
(11) undefined. http://proxy.example.com:80/.