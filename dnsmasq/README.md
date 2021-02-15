## dnsmasq

use dnsmasq to return DNS records, include srv.

```shell
$ docker-compose -d up
$ dig -t SRV @localhost srv.test.local

; <<>> DiG 9.16.1-Ubuntu <<>> -t SRV @localhost srv.test.local
; (1 server found)
;; global options: +cmd
;; Got answer:
;; WARNING: .local is reserved for Multicast DNS
;; You are currently testing what happens when an mDNS query is leaked to DNS
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 36200
;; flags: qr aa rd ra ad; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 2

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;srv.test.local.                        IN      SRV

;; ANSWER SECTION:
srv.test.local.         0       IN      SRV     0 100 80 10-0-53-61.srv.test.local.

;; ADDITIONAL SECTION:
10-0-53-61.srv.test.local. 0    IN      A       10.0.53.61

;; Query time: 1 msec
;; SERVER: 127.0.0.1#53(127.0.0.1)
;; WHEN: Tue Feb 16 08:55:21 JST 2021
;; MSG SIZE  rcvd: 104
```