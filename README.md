nginx_tcp_struct
================

build your nginx in normal way, here is one example：

    server {
        listen       8123;
        server_name  localhost;
        default_head "GET /demo.html HTTP/1.1\r\nHost: 172.22.31.53\r\n\r\n";

        location /demo.html {
            content_by_lua_file conf/demo.lua;
        }
    }

conf/demo.lua code：

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


version
================
if you need special version, please choose the branch or merge it by yourself.

provide by membphis@gmail.com

if you have any question, please let me know. 