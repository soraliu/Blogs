Nginx 配置
==================


# 安装nginx

    brew install nginx

# 配置nginx

配置文件: **/usr/local/etc/nginx/nginx.conf**

+ 将 __http__ 部分修改成下面的配置

        http {
            include       /etc/nginx/mime.types;
            default_type  application/octet-stream;
        
            log_format    main  '$remote_addr - $remote_user [$time_local] "$request" '
                                '$status $body_bytes_sent "$http_referer" '
                                '"$http_user_agent" "$http_x_forwarded_for"';
        
            access_log    /var/log/nginx/access.log  main;    #日志文件存放位置
        
            sendfile      on;
        
            keepalive_timeout  65;
        
            gzip on;
            gzip_http_version  1.0;
            gzip_disable       "MSIE [1-6].";
            gzip_min_length    512;
            gzip_types         text/plain application/x-javascript text/css text/javascript application/javascript application/xml application/json;
        
            include servers/*.conf;
        
        }

    > 如果日志文件的路径不存在, 则需要先创建: sudo mkdir -p /var/log/nginx/
    
    > 修改访问权限: sudo chmod -R a+rw /var/log/nginx/
    
    > 如果出现没有权限访问access.log文件, 可手工创建, 并修改权限:
     
    > touch /var/log/nginx/access.log && sudo chmod a+rw /var/log/nginx/access.log

## 代理配置

配置文件: **/usr/local/etc/nginx/proxy.conf**

+ proxy.conf的内容如下:

        proxy_redirect          off;
        proxy_set_header        Host $host;
        proxy_set_header        X-Real-IP $remote_addr;
        client_max_body_size    10m;
        client_body_buffer_size 128k;
        proxy_connect_timeout   300;
        proxy_send_timeout      300;
        proxy_read_timeout      300;
        proxy_buffer_size       4k;
        proxy_buffers           4 32k;
        proxy_busy_buffers_size 64k;
        proxy_temp_file_write_size 64k;

## 配置转发

配置文件: **/usr/local/etc/nginx/servers/food.conf**

**注意**: 

请将配置文件中的 **/root/workspace/com-petkit-food-web/src** 修改为本地路径

    #测试服务器
    upstream sandbox_petkit_food {
        server sandbox.food.petkit.com:80;
    }
    
    #正式服务器
    upstream petkit_food {
        server food.petkit.com;
    }
    
    server {
         listen 80;
         server_name  localhost 127.0.0.1; #可通过这二个地址访问
         
         #日志文件的存放位置
         access_log    /var/log/nginx/food-access.log  main;
         error_log     /var/log/nginx/food-error.log;
         
         index index.html;
         
         location /food/api {
            proxy_pass              http://petkit_food/api;
            proxy_set_header        Host food.petkit.com; #正式服务器对Host有限定
            include proxy.conf;
         }
         
         location /food/admin {
            proxy_pass              http://petkit_food/admin;
            proxy_set_header        Host food.petkit.com; #正式服务器对Host有限定
            include proxy.conf;
         }
         
         location /food {
            alias       /root/workspace/com-petkit-food-web/src;  #将该地址指向正确的项目路径
            index       index.html index.htm;
         }
         
         location /api {
            proxy_pass              http://sandbox_petkit_food/api;
            proxy_set_header        Host sandbox.food.petkit.com; #测试服务器对Host有限定
            include proxy.conf;
         }
         
         location /admin {
            proxy_pass              http://sandbox_petkit_food/admin;
            proxy_set_header        Host sandbox.food.petkit.com; #测试服务器对Host有限定
            include proxy.conf;
         }
         
         location / {
            root        /root/workspace/com-petkit-food-web/src;  #将该地址指向正确的项目路径
            index       index.html index.htm;
         }
         
         location @badrequest {
             default_type "application/json; charset=utf-8";
             if ($http_x_locale ~* "^zh") {
               return 400 "{\"error\":{\"code\":96,\"message\":\"请求不正确\"}}";
             }
             return 400 "{\"error\":{\"code\":96,\"msg\":\"Bad request\"}}";
         }
         location @versiontooold {
             default_type "application/json; charset=utf-8";
             if ($http_x_locale ~* "^zh") {
               return 404 "{\"error\":{\"code\":97,\"message\":\"App版本过低，请升级\"}}";
             }
             return 404 "{\"error\":{\"code\":97,\"message\":\"App is too old, please upgrade\"}}";
         }
         location @servererror {
             default_type "application/json; charset=utf-8";
             if ($http_x_locale ~* "^zh") {
               return 500 "{\"error\":{\"code\":98,\"message\":\"系统繁忙，请稍后重试\"}}";
             }
             return 500 "{\"error\":{\"code\":98,\"message\":\"Unknown error, please retry later\"}}";
         }
         location @maintenance{
             default_type "application/json; charset=utf-8";
             if ($http_x_locale ~* "^zh") {
               return 503 "{\"error\":{\"code\":99,\"message\":\"很抱歉，服务器维护中，请稍后重试\"}}";
             }
             return 503 "{\"error\":{\"code\":99,\"message\":\"The server is under maintenance, please retry later\"}}";
         }
         error_page 400 = @badrequest;
         error_page 404 = @versiontooold;
         error_page 500 = @servererror;
         error_page 502 503 504 = @maintenance;
    }


# 验证配置文件

    nginx -t

# 启动nginx

    nginx

# 重启或重加载配置文件

    nginx -s reload


# 访问

浏览器输入 

+ 访问测试接口环境

        localhost/

+ 访问正式接口环境

        localhost/food/



