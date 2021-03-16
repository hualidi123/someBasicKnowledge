# ip_local_port_range=6000 6001
> 首先这个ip_local_port_range 参数代表作为客户端发起连接时可以随机使用的端口范围
> 上面的随机端口是2个，代表同时对同一个目标主机的同一个端口只能有两个连接，这个符合<协议，客户端ip,客户端port,目标主机ip，目标主机port>来标识唯一sock连接的原理

```shell
$ nohup nc 123.125.114.144 80 -v &
$ nohup nc 123.125.114.144 80 -v &
$ nc 123.125.114.144 80 -v
nc: connect to 123.125.114.144 port 80 (tcp) failed: Cannot assign requested address
$ nohup nc 123.125.114.144 443 -v &
$ nohup nc 123.125.114.144 443 -v &
$ nc 123.125.114.144 443 -v
nc: connect to 123.125.114.144 port 443 (tcp) failed: Cannot assign requested address
$ nohup nc 220.181.57.216 80 -v &
$ nohup nc 220.181.57.216 80 -v &
$ nc 220.181.57.216 80 -v
nc: connect to 220.181.57.216 port 80 (tcp) failed: Cannot assign requested address
$ nohup nc 220.181.57.216 443 -v &
$ nohup nc 220.181.57.216 443 -v &
$ nc 220.181.57.216 443 -v
nc: connect to 220.181.57.216 port 443 (tcp) failed: Cannot assign requested address
$ ss -ant |grep 10.0.2.15:61
SYN-SENT  0        1               10.0.2.15:61001       220.181.57.216:80
ESTAB     0        0               10.0.2.15:61001      123.125.114.144:443
ESTAB     0        0               10.0.2.15:61000       220.181.57.216:443
SYN-SENT  0        1               10.0.2.15:61000       220.181.57.216:80
SYN-SENT  0        1               10.0.2.15:61001      123.125.114.144:80
ESTAB     0        0               10.0.2.15:61000      123.125.114.144:443
SYN-SENT  0        1               10.0.2.15:61000      123.125.114.144:80
ESTAB     0        0               10.0.2.15:61001       220.181.57.216:443
```
