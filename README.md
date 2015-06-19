nginx-tcp-lua-server
================

How to build it
================
```
1. download the special version from openresty.com
    eg: wget http://openresty.org/download/ngx_openresty-1.7.10.1.tar.gz
2. download the special version patch
3. make and install 
```

How to use it
================
Here is a nginx config example：

```
    server {
        listen       8123;
        server_name  localhost;
        default_head "GET /demo.html HTTP/1.1\r\nHost: 172.22.31.53\r\n\r\n";

        location /demo.html {
            content_by_lua '
                local tcp_req, err = ngx.req.socket(true)
                if not tcp_req then
                    ngx.log(ngx.ERR, "get req socket err: ", err)
                end
                 
                while true do
                    local req_data
                    req_data, err = tcp_req:receive(1)
                    if not req_data then
                        ngx.log(ngx.ERR, "get req_data err: ", err)
                        return
                    end
                 
                    ngx.log(ngx.ERR, "req data:", req_data)
                    tcp_req:send(req_data)
                end
            ';
        }
    }
```

Pay attention
================
Its not a pure nginx tcp server. But is more power if you use ngx-lua with this way. It have great compatibility, and you can use 100% api of nginx-lua-module.

provide by membphis@gmail.com

if you have any question, please let me know. 
