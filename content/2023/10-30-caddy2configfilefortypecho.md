---
date: 2023-10-30
title: Caddy2 的 typecho 伪静态配置
author: Jn
categories: 工作
---

想用 caddy2 做反向代理 typecho 1.2.1，网上搜的伪静态的配置文件都是 caddy1的或者 Nginx的。
找来找去自己拼凑了一套。

```
# /etc/caddy/Caddyfile
blog.grav.cn {
        encode gzip
        root * /data/typecho/
        @static {
                path *.js
                path *.css
                path *.jpg
                path *.png
                path *.ico
                path *.svg
                path *.ttf
                path *.woff
                path *.woff2
                path *.xsl
        }
        file_server @static {
                hide .git
                hide bloggrav.db
        }
        @unstatic {
                not path /admin*
                not path /robots.txt
                path */
        }
        rewrite @unstatic /index.php
        php_fastcgi 172.18.0.7:9000 {
                root /app
        }
}
```

9000 端口跑的是 typecho 的镜像 `joyqi/typecho:1.2.1-php8.0-fpm-alpine`
