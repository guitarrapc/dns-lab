## Coredns

```shell
$ docker-compose up
$ dig @localhost server-a.test.local
$ dig @localhost server-a.example
```

coredns + etcd to manage srv.

```shell
$ docker-compose exec etcd /bin/sh
# etcdctl put /skydns/test/hoge '{"host":"192.168.1.5","port":8080}'
# etcdctl get /skydns/test/hoge
# exit
$ dig @localhost A hoge.test +noall +answer
hoge.test.              300     IN      A       192.168.1.5
```

```shell
$ docker-compose exec etcd /bin/sh
# ETCDCTL_API=3 etcdctl put /skydns/local/example/_tcp/_web/web-server01 '{"host":"1.1.1.1","port":80}'
# ETCDCTL_API=3 etcdctl put /skydns/local/example/_tcp/_web/web-server02 '{"host":"1.1.1.2","port":80}'
# ETCDCTL_API=3 etcdctl put /skydns/local/example/_tcp/_web/web-server03 '{"host":"1.1.1.3","port":80}'
$ dig @localhost -t SRV _web._tcp.example.local
; <<>> DiG 9.16.1-Ubuntu <<>> @localhost -t SRV _web._tcp.example.local
; (1 server found)
;; global options: +cmd
;; Got answer:
;; WARNING: .local is reserved for Multicast DNS
;; You are currently testing what happens when an mDNS query is leaked to DNS
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 287
;; flags: qr aa rd; QUERY: 1, ANSWER: 3, AUTHORITY: 0, ADDITIONAL: 4
;; WARNING: recursion requested but not available

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
; COOKIE: 0edad208a650a341 (echoed)
;; QUESTION SECTION:
;_web._tcp.example.local.       IN      SRV

;; ANSWER SECTION:
_web._tcp.example.local. 300    IN      SRV     10 33 80 web-server01._web._tcp.example.local.
_web._tcp.example.local. 300    IN      SRV     10 33 80 web-server02._web._tcp.example.local.
_web._tcp.example.local. 300    IN      SRV     10 33 80 web-server03._web._tcp.example.local.

;; ADDITIONAL SECTION:
web-server01._web._tcp.example.local. 300 IN A  1.1.1.1
web-server02._web._tcp.example.local. 300 IN A  1.1.1.2
web-server03._web._tcp.example.local. 300 IN A  1.1.1.3

;; Query time: 1 msec
;; SERVER: 127.0.0.1#53(127.0.0.1)
;; WHEN: Tue Feb 16 09:49:03 JST 2021
;; MSG SIZE  rcvd: 457
$ dig +short @localhost -t A _web._tcp.example.local
1.1.1.1
1.1.1.2
1.1.1.3
```

## REF

> * Corefile: https://coredns.io/2017/07/23/corefile-explained/