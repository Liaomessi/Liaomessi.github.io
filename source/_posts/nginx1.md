---
title: nginx系列(一)
catalog: true
date: 2021-01-11 22:52:03
subtitle:
header-img:
tags: 
  - nginx
catagories:
- nginx
top: 2
---

## 安装证书全流程
{% asset_img image-20201106211115329.png 安装证书流程图 %}

### 下载证书压缩包并解压，生成证书的key和pem

- 如下图是我在阿里云的证书，点击下载。

{% asset_img image-20201106212028665.png 阿里云证书 %}

- 选择你需要的类型，我使用nginx，下载了nginx的版本。

{% asset_img image-20201106212102055.png nginx证书 %}

- 解压证书压缩包，生成key和 pem，如下图所示。上传至服务器即可，服务器路径可随意，记住路径即可。

{% asset_img image-20201106212349144.png 证书上传服务器 %}

### 配置nginx.conf使ssl证书生效

```xml server {	
    listen 443 ssl;      #增加ssl访问支持
    server_name  www.demo.com demo.com;
    root         /demo/dict;
    ssl_certificate ${你的.pem文件的路径};   #将domain name.pem替换成您证书的文件名。
    ssl_certificate_key ${你的.key文件的路径};   #将domain name.key替换成您证书的密钥文件名。
    ssl_session_timeout 5m;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;  #使用此加密套件。
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;   #使用该协议进行配置。
    ssl_prefer_server_ciphers on;  
    location / {
         root /demo/dict; 
         index index.html;
    }
}
```

### 配置http重写路由至https

```xml server{
 listen       80;
 server_name  www.demo.com demo.com;
 rewrite ^(.*) https://$host$1 permanent;
}
```

...
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=530 height=290 src="//music.163.com/outchain/player?type=0&id=3150640120&auto=0&height=430"></iframe>
...