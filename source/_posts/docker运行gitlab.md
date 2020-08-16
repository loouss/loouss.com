---
title: docker运行gitlab
date: 2020-08-09 22:25:10
tags:
---

~~~shell

docker run --detach --restart always --hostname gitlab.xx.xx --publish 127.0.0.1:8080:80 --publish 0.0.0.0:22:22 --name gitlab --volume /application/gitlab/logs:/var/log/gitlab --volume /application/gitlab/data:/var/opt/gitlab --volume /application/gitlab/config:/etc/gitlab gitlab/gitlab-ce

~~~

> 如果存在版本升级情况，请按照[升级路径](https://docs.gitlab.com/ee/policy/maintenance.html#upgrading-major-versions)升级

~~~

server {
    listen       443 ssl http2;
    listen       [::]:443 ssl http2;
    server_name  xxx.xx.xx;

    ssl_certificate "/application/ssl/xxx.xx.xx/xxx.pem";
    ssl_certificate_key "/application/ssl/xxx.xx.xx/xxx.key";

    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers ALL:!ADH:!EXPORT56:RC4+RSA:+HIGH:+MEDIUM:+LOW:+SSLv2:+EXP;
    ssl_prefer_server_ciphers on;
    ssl_session_timeout  10m;

    location / {
        proxy_pass http://127.0.0.1:8080;
    }

}

~~~