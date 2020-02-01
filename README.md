## 目的
这个仓库用于在阿里云函数计算上搭建DoH服务器

## DNS解析流程
1. 本地客户端向函数计算发送DNS解析请求
2. 函数计算服务端，首先由`Caddy`进行代理，Caddy监听9000端口（函数计算要求），Caddy对`path`进行`rewrite`，然后反向代理到8053端口上
3. 8053端口由`CoreDNS`监听，CoreDNS将请求`forward`到`1.1.1.1`(TLS协议)，需要自定义解析的，可在配置文件自行配置，例如本仓库使用host文件进行自定义解析，需要对区域进行解析的，可以参考相关[文档](https://coredns.io/)
4. 返回结果

## 目的
1. 安全，防止DNS污染
2. 自定义解析，可自定义对某些区域的解析，也可单独对某域名解析

## 使用
1. 使用[`fun`命令行工具](https://github.com/alibaba/funcraft)进行部署，编辑`template.yml`文件，具体可参考相关[文档](https://github.com/alibaba/funcraft/blob/master/docs/specs/2018-04-03-zh-cn.md)
2. 本地部署[`dnscrypt-proxy`](https://github.com/DNSCrypt/dnscrypt-proxy)客户端，编辑`dnscrypt-proxy.toml`配置文件，主要[修改解析源](https://github.com/DNSCrypt/dnscrypt-proxy/wiki/Configuration-Sources)
   1. `server_names`修改为myserver
   2. 在`static`中添加`myserver`，然后修改stamp，计算方法[在这](https://dnscrypt.info/stamps/)
3. 本地修改DNS解析服务器为`127.0.0.1`，然后就可以愉快的玩耍了