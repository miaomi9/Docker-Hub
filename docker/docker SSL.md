# acme域名证书脚本

[官方文档](https://github.com/acmesh-official/acme.sh/wiki/%E8%AF%B4%E6%98%8E)

## 安装脚本
 **官方脚本：** 
```
apt install socat curl
```
```
curl https://get.acme.sh | sh
```



## 使用DNS申请

如果是阿里云平台：登录控制台，在 `Access Key` 管理页面创建 `Access Key` 和 `Access Secret`。将下面命令中的`dns_cf`改为`dns_aliyun`然后根据提示输入`Access Key` 和 `Access Secret`。

 **配置Cloudflare插件：** 
```
~/.acme.sh/acme.sh --set-default-ca --server letsencrypt --dns dns_cf
```

 **申请证书：**
```
export CF_Key="你的api key"
export CF_Email="你的邮箱"
```
```
~/.acme.sh/acme.sh --issue --dns dns_cf -d 你的域名
```



## 使用HTTP方式申请
```
~/.acme.sh/acme.sh --issue --standalone -d 你的域名 --server letsencrypt
```

## 安装证书(可选)
```
~/.acme.sh/acme.sh --install-cert -d 你的域名 --cert-file /etc/ssl/cert.pem --key-file /etc/ssl/key.pem --fullchain-file /etc/ssl/fullchain.pem
```
可以在结尾添加`--reloadcmd "service nginx reload`直接配置到nginx


### win系统申请证书

[进入win系统acme官网](https://www.win-acme.com/)

下载程序，执行exe文件，按照提示操作


# docker一键申请

```
services:
  https-portal:
    image: steveltn/https-portal:1
    ports:
      - '80:80'
      - '443:443'
    environment:
      DOMAINS: 'example.com' # 你的域名
      STAGE: 'production' # 生产环境
      FORCE_RENEW: 'true' # 证书续订
    volumes:
      - https-portal-data:/var/lib/https-portal
      - /var:/var/lib/https-portal # 映射文件到宿主机

volumes:
  https-portal-data:
```


### dns方式申请

```
services:
  https-portal:
    image: steveltn/https-portal:1
    ports:
      - '80:80'
      - '443:443'
    environment:
      DOMAINS: 'example.com' # 你的域名
      STAGE: 'production'
      FORCE_RENEW: 'true' # 证书续订
      DNS_PROVIDER: 'cloudflare' # 或 'alidns'
      CLOUDFLARE_EMAIL: 'your-email@example.com' # 对应于Cloudflare
      CLOUDFLARE_API_KEY: 'your-cloudflare-api-token' # 对应于Cloudflare
      # ALI_KEY: 'your-ali-access-key-id' # 对应于阿里云
      # ALI_SECRET: 'your-ali-access-key-secret' # 对应于阿里云
    volumes:
      - https-portal-data:/var/lib/https-portal
      - /var:/var/lib/https-portal # 映射文件到宿主机

volumes:
  https-portal-data:
```
