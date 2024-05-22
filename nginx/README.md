- [回到首页](../README.md)

## 官网

[打开官网](https://nginx.org/)

## nginx-sticky配置

1. 下载nginx和sticky插件
    
    - [nginx1.9.9下载](http://nginx.org/download/nginx-1.9.9.tar.gz)    
    - [nginx-sticky下载](https://bitbucket.org/nginx-goodies/nginx-sticky-module-ng/get/master.tar.gz)

2. 解压压缩包
    
    ```cmd
    
    tar -zxf nginx-1.9.9.tar.gz
    
    tar -zxf master.tar.gz
    
    mv nginx-goodies-nginx-sticky-module-ng-08a395c66e42 nginx-sticky
    
    ```
   
3. 安装pcre依赖库

    ```cmd
    apt-get install libpcre3 libpcre3-dev
    ```

4. 安装nginx
    
    ```cmd
    cd nginx-1.9.9
    
    ./configure  --with-http_gzip_static_module --with-http_flv_module --with-http_dav_module --with-http_stub_status_module --with-http_realip_module --add-module=/home/nginx-sticky/
    
    make
    
    make install
    ```
5. 配置nginx+sticky
    
    ```cmd
    cd /usr/local/nginx/conf/
    
    vi nginx.conf
    ```
   
    ```cmd
    http {
        upstream test {
            sticky;
            server 127.0.0.1:8080;
            server 127.0.0.1:8081;
        }
        server {
            location / {
                charset utf-8;
                proxy_pass http://test/;
            }
        }
    }
    ```
   
6. 启动nginx

    ```cmd
    /usr/local/nginx/sbin/nginx
    ```

7. 查询nginx进程

    ```cmd
    ps -ef | grep nginx
    ```

***

## ubuntu配置登录问候语

```cmd
vi /etc/motd
```
