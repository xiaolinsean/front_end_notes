- [`1、常用命令`](#1常用命令)
- [`2、常见基本配置`](#2常见基本配置)
- [`3、设置反向代理`](#3设置反向代理)
- [`4、负载均衡`](#4负载均衡)
- [`5、负载均衡的几种方式`](#5负载均衡的几种方式)
- [`6、设置长连接`](#6设置长连接)
- [`7、TIME_WAIT问题`](#7time_wait问题)
- [`7、设置缓存`](#7设置缓存)

## `1、常用命令`

- 查看nginx版本(小写字母v)
    ```shell
    nginx -v
    ```
- 除版本信息外还显示配置参数信息(大写字母V)
    ```shell
    nginx -V
    ```

- 启动nginx
    ```shell
    start nginx
    ``` 

- 指定配置文件启动nginx
    ```shell
    start nginx -c filename
    ```

- 关闭nginx，完整有序的停止nginx，保存相关信息
    ```shell
    nginx -s quit
    ```

- 关闭nginx，快速停止nginx，可能并不保存相关信息
    ```shell
    nginx -s stop
    ```

- 重新载入nginx，当配置信息修改需要重新加载配置是使用
    ```shell
    nginx -s reload
    ```

- 重新打开日志文件
    ```shell
    nginx -s reopen
    ```

- 测试nginx配置文件是否正确
    ```shell
    nginx -t -c filename
    ```

## `2、常见基本配置`
```shell
########### 每个指令必须有分号结束。#################
#user administrator administrators;  #配置用户或者组，默认为nobody nobody。
#worker_processes 2;  #允许生成的进程数，默认为1
#pid /nginx/pid/nginx.pid;   #指定nginx进程运行文件存放地址
error_log log/error.log debug;  #制定日志路径，级别。这个设置可以放入全局块，http块，server块，级别以此为：debug|info|notice|warn|error|crit|alert|emerg
events {
    accept_mutex on;   #设置网路连接序列化，防止惊群现象发生，默认为on
    multi_accept on;  #设置一个进程是否同时接受多个网络连接，默认为off
    #use epoll;      #事件驱动模型，select|poll|kqueue|epoll|resig|/dev/poll|eventport
    worker_connections  1024;    #最大连接数，默认为512
}
http {
    include       mime.types;   #文件扩展名与文件类型映射表
    default_type  application/octet-stream; #默认文件类型，默认为text/plain
    #access_log off; #取消服务日志    
    log_format myFormat '$remote_addr–$remote_user [$time_local] $request $status $body_bytes_sent $http_referer $http_user_agent $http_x_forwarded_for'; #自定义格式
    access_log log/access.log myFormat;  #combined为日志格式的默认值
    sendfile on;   #允许sendfile方式传输文件，默认为off，可以在http块，server块，location块。
    sendfile_max_chunk 100k;  #每个进程每次调用传输数量不能大于设定的值，默认为0，即不设上限。
    keepalive_timeout 65;  #连接超时时间，默认为75s，可以在http，server，location块。

    upstream mysvr {   
      server 127.0.0.1:7878;
      server 192.168.10.121:3333 backup;  #热备
    }
    error_page 404 https://www.baidu.com; #错误页
    server {
        keepalive_requests 120; #单连接请求上限次数。
        listen       4545;   #监听端口
        server_name  127.0.0.1;   #监听地址       
        location  ~*^.+$ {       #请求的url过滤，正则匹配，~为区分大小写，~*为不区分大小写。
           #root path;  #根目录
           #index vv.txt;  #设置默认页
           proxy_pass  http://mysvr;  #请求转向mysvr 定义的服务器列表
           deny 127.0.0.1;  #拒绝的ip
           allow 172.18.5.54; #允许的ip           
        } 
    }
}
```

- 1、**全局块**
  
    配置影响nginx全局的指令。一般有运行nginx服务器的用户组，nginx进程pid存放路径，日志存放路径，配置文件引入，允许生成worker process数等。
    
- 2、**events块**
  
    配置影响nginx服务器或与用户的网络连接。有每个进程的最大连接数，选取哪种事件驱动模型处理连接请求，是否允许同时接受多个网路连接，开启多个网络连接序列化等。

- 3、**http块**
  
    可以嵌套多个server，配置代理，缓存，日志定义等绝大多数功能和第三方模块的配置。如文件引入，mime-type定义，日志自定义，是否使用sendfile传输文件，连接超时时间，单连接请求数等。

- 4、**server块**
  
    配置虚拟主机的相关参数，一个http中可以有多个server。
    
- 5、**location块**
  
    配置请求的路由，以及各种页面的处理情况。

## `3、设置反向代理`

- **正向代理**：
    正向代理代理的是客户端，代理器代替客户端向服务端发起请求，并将请求的结果返回给客户端，对于服务端来说，并不知道客户端的存在；而对于客户端来说，明确知道请求的服务端；

- **反向代理**：
    正向代理代理的是服务端，代理器将客户端的请求转发给对应的服务，并将请求的结果返回给客户端，对于服务端来说，知道客户端的存在；而对于客户端来说，并不知道请求具体发送到哪个服务上。

```shell
server {
	listen       80;
	server_name  127.0.0.1;

	location ~ /edu/ {
		proxy_pass  http://127.0.0.1:9000;
	}

	location ~ /vod/ {
		proxy_pass  http://127.0.0.1:9001;
	}
}
```

设置反向代理主要使用 `location` 块中的 `proxy_pass`，将 `location` 匹配到的请求转发到 `proxy_pass` 设置的服务上。

## `4、负载均衡`

负载均衡主要通过 `upstream` 模块来定义，`upstream` 模块中通过 `server` 来枚举服务器列表，在 `upstream` 模块配置完成后，要让指定的访问反向代理到服务器列表，基本配置如下：

```shell
http {
    upstream myserverlist {
        server 192.168.22.1;
        server 192.168.22.2;
    }
    server {
        listen 80;
        location / {
            proxy_pass http://myserverlist;
        }
    }
}
```

## `5、负载均衡的几种方式`

内置的负载均衡的执行策略有四种：轮询策略（默认负载均衡策略）、最少连接数负载均衡策略（least_conn）、ip-hash 负载均衡策略和权重负载均衡策略（weight）

- 轮询策略（默认负载均衡策略）
  
    最基本的配置方法，它是upstream模块默认的负载均衡默认策略。每个请求会按时间顺序逐一分配到不同的后端服务器。

    - 在轮询中，如果服务器down掉了，会自动剔除该服务器。
    - 此策略适合服务器配置相当，无状态且短平快的服务使用。
    
    每个 server 可设置的参数有：
    - fail_timeout: 与max_fails结合使用。
    - max_fails: 设置在fail_timeout参数设置的时间内最大失败次数，如果在这个时间内，所有针对该服务器的请求都失败了，那么认为该服务器会被认为是停机了，
    - fail_time: 服务器会被认为停机的时间长度,默认为10s。
    - backup: 标记该服务器为备用服务器。当主服务器停止时，请求会被发送到它这里。
    - down:	标记服务器永久停机了。

    ```shell
    upstream myserverlist {
        server 192.168.22.1 max_fails=3 fail_timeout=20s;
        server 192.168.22.2 backup;
    }
    ```
  
- 最少连接数负载均衡策略（least_conn）
    
    把请求转发给连接数较少的后端服务器。轮询算法是把请求平均的转发给各个后端，使它们的负载大致相同；但是，有些请求占用的时间很长，会导致其所在的后端负载较高。这种情况下，least_conn这种方式就可以达到更好的负载均衡效果。

    - 此负载均衡策略适合请求处理时间长短不一造成服务器过载的情况

    ```shell
    upstream myserverlist {
        least_conn;
        server 192.168.22.1;
        server 192.168.22.2;
    }
    ```

- ip_hash 负载均衡策略
  
    按照基于客户端IP的分配方式，这个方法确保了相同的客户端的请求一直发送到相同的服务器，以保证session会话。这样每个访客都固定访问一个后端服务器，可以解决session不能跨服务器的问题。

    - 在nginx版本1.3.1之前，不能在ip_hash中使用权重（weight）。
    - ip_hash不能与backup同时使用。
    - 此策略适合有状态服务，比如session。
    - 当有服务器需要剔除，必须手动down掉。

    ```shell
    upstream myserverlist {
        ip_hash;
        server 192.168.22.1;
        server 192.168.22.2;
    }
    ```

- 权重负载均衡策略（weight）
    
    在负载均衡的基础加上均衡概率，weight参数用于指定访问几率，weight的默认值为1,；weight的数值与访问比率成正比，

    - 权重越高分配到需要处理的请求越多。
    - 此策略可以与least_conn和ip_hash结合使用。
    - 此策略比较适合服务器的硬件配置差别比较大的情况。

    ```shell
    upstream myserverlist {
        least_conn;
        server 192.168.22.1 weight=2;
        server 192.168.22.2;
    }
    ```

除了上述的四个内置的策略外，还有些其他第三方插件提供的均衡策略：

- Fail (第三方插件)

    按后端服务器的响应时间来分配请求，响应时间短的优先分配。该策略请求转发到负载最小的后端服务器节点上。Nginx通过后端服务器节点对响应时间来判断负载情况，响应时间最短的节点负载就相对较轻，Nginx就会将前端请求转发到此后端服务器节点上。

    ```shell
    upstream myserverlist {
        fair;
        server 192.168.22.1 weight=2;
        server 192.168.22.2;
    }
    ```
    
- url_hash（第三方插件）
    
    按访问url的hash结果来分配请求，使每个url定向到同一个后端服务器，后端服务器为缓存时比较有效。
    在upstream中加入hash语句，hash_method是使用的hash算法。

    ```shell
    upstream myserverlist {
        hash $request_uri;//非域名后面的资源部分
        hash_method crc32;
        server 192.168.22.1 weight=2;
        server 192.168.22.2;
    }
    ```

- Sticky策略
    
    该策略在多台服务器的环境下，为了确保一个客户端只和一台服务器通讯，它会保持长连接，并在结束会话后再次选择一个服务器，保证了压力均衡。

    ```shell
    upstream myserverlist {
        sticky expires=1h;
        server 192.168.22.1 weight=2;
        server 192.168.22.2;
    }
    ```
    Sticky策略一般用于 ip_hash 策略不可处理的场景，且与 ip_hash 不可同时使用。

    通常，Sticky策略检查每一个请求是否有对应的标识，这些标识符通常在HTTP cookie中传递。如果一个请求包含已经包含了会话标识符，则NGINX Plus将请求转发到该会话上一个请求转发到的服务器；如果一个请求中没有会话标识符，说明这是一个新的会话，则 NGINX 会根据其他策略将请求转发的任意服务器，并加上对应的会话标识符。


## `6、设置长连接`

当使用nginx作为反向代理时，为了支持长连接，需要做到两点：

- 从client到nginx的连接是长连接
- 从nginx到server的连接是长连接

从HTTP协议的角度看，nginx在这个过程中，对于客户端它扮演着HTTP服务器端的角色。而对于真正的服务器端（在nginx的术语中称为upstream）nginx又扮演着HTTP客户端的角色。

- ### 保持client到nginx的长连接

    默认情况下，nginx已经自动开启了对client连接的keep alive支持，但通常会有两个相关参数可以设置：

    ```shell
    http {
        keepalive_timeout  60s 60s;
        keepalive_requests 1000;
    }
    ```

    - keepalive_timeout

        keepalive_timeout参数设置keep-alive客户端连接在服务器端保持开启的超时值。值为0会禁用keep-alive客户端连接,默认是75s。默认75s一般情况下也够用，对于一些请求比较大的内部服务器通讯的场景，适当加大为120s或者300s。
        
        可选的第二个参数在响应的header域中设置一个值“Keep-Alive: timeout=time”。这两个参数可以不一样,第二个参数通常可以不用设置。

    - keepalive_requests

        keepalive_requests 指令用于设置一个keep-alive连接上可以服务的请求的最大数量。当最大请求数量达到时，连接被关闭。默认是100。

        大多数情况下当QPS(每秒请求数)不是很高时，默认值100凑合够用。但是，对于一些QPS比较高（比如超过10000QPS甚至更高) 的场景，默认的100就显得太低。因为 QPS 比较高的时候， keepalive_requests 很容易达到其上限值，这样就会不停的出现长连接被关闭，新的长连接被建议的情况，这时 Nignx 端就出现大量的 TIME_WAIT。

- ### 保持 nginx 到 server 的长连接

    ```shell
    http {
        upstream myserverlist {
            ...
            keepalive 1024;
        }
        server {
            listen 80;
            location / {
                proxy_pass http://myserverlist;

                proxy_http_version 1.1;                    # 这两个很重要
                proxy_set_header Connection "";
            }
        }
    }
    ```

    - location 中的设置

        HTTP协议中对长连接的支持是从1.1版本之后才有的，默认的 http 1.0 是短连接；
        重置 `Connection header` 应该被清理，这样就可以保证不论 client 和 Nginx 是否是长连接， Nginx 和 server 都可以开启长连接。

    - upstream 的设置

        keepalive: 设置到 upstream 服务器的空闲 keepalive 连接的最大数量，当空闲的 keepalive 的连接数量超过这个值时，最近使用最少的连接将被关闭，如果这个值设置得太小的话，某个时间段请求数较多，而且请求处理时间不稳定的情况，建立的长连接处理完请求后，被放置到长连接池里，等待下一批请求，但是因为 `keepalive` 值太小了，长连接池里的空闲长连接很容易超过该值，就会关闭最近使用最少的连接，这样下一批请求过来后，就需要新建长连接，这样就可能会出现不停的关闭和创建长连接数。我们一般设置 1024 即可。特殊场景下，可以通过接口的平均响应时间和QPS估算一下。


## `7、TIME_WAIT问题`

通过以下语句可以查看对应数量：

```shell
netstat -n | awk '/^tcp/ {++S[$NF]} END {for(a in S) print a, S[a]}'
```

发起tcp四次挥手的一方将会进入TIME_WAIT状态，而TIME_WAITE的等待时间默认是2MSL，MSL默认是2min

- （1）导致 nginx端出现大量TIME_WAIT的情况有两种：

    - keepalive_requests设置比较小，高并发下超过此值后nginx会强制关闭和客户端保持的keepalive长连接；（主动关闭连接后导致nginx出现TIME_WAIT）

    - keepalive设置的比较小（空闲数太小），导致高并发下nginx会频繁出现连接数震荡（超过该值会关闭连接），不停的关闭、开启和后端server保持的keepalive长连接；

- （2）导致后端server端出现大量TIME_WAIT的情况：

    nginx没有打开和后端的长连接，即：没有设置proxy_http_version 1.1;和proxy_set_header Connection “”;从而导致后端server每次关闭连接，高并发下就会出现server端出现大量TIME_WAIT
    

## `7、设置缓存`

