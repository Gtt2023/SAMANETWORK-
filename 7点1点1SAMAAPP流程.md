# 7.1.9  SAMA APP流程
九.
1.privoxy 启动监听127.0.0.1:8888
       说明:HTTP代理,可将HTTP数据包转化成socks5数据格式
       配置conf
listen-address 127.0.0.1:8888
forward-socks5 / 127.0.0.1:9999 .
2.SamaAPP启动监听 127.0.0.1:9999
3.流程测试
(1)curl -x 127.0.0.1:8888 https://www.baidu.com
(2)privoxy与SamaAPP协商将HTTP请求转化成socks5协议数据后发送到SamaAPP
(3)SamaAPP收到socks5数据后,用aes-128-cfb算法,APP PUBKEY和WORKER PUBKEY计算得到的ECDH KEY作为密码,进行数据加密,得到的数据作为BODY
(4)组包SamaHead(具体查看组包模块)
(5)发送SamaAPP Head和BODY到AUDITOR
(6)后面只需要将实际数据加密发送即可,不需要发送SamaAPP HEAD
