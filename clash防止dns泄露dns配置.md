```
dns:
  enable: true
  ipv6: true
  enhanced-mode: fake-ip
  listen: 53
  fake-ip-range: 198.18.0.1/16
  fake-ip-filter:
  - '*.lan'
  - '*.localdomain'
  - '*.example'
  - '*.invalid'
  - '*.localhost'
  - '*.test'
  - '*.local'
  - '*.home.arpa'
  - time.*.com
  - time.*.gov
  - time.*.edu.cn
  - time.*.apple.com
  - time1.*.com
  - time2.*.com
  - time3.*.com
  - time4.*.com
  - time5.*.com
  - time6.*.com
  - time7.*.com
  - ntp.*.com
  - ntp1.*.com
  - ntp2.*.com
  - ntp3.*.com
  - ntp4.*.com
  - ntp5.*.com
  - ntp6.*.com
  - ntp7.*.com
  - '*.time.edu.cn'
  - '*.ntp.org.cn'
  - +.pool.ntp.org
  - time1.cloud.tencent.com
  - music.163.com
  - '*.music.163.com'
  - '*.126.net'
  - musicapi.taihe.com
  - music.taihe.com
  - songsearch.kugou.com
  - trackercdn.kugou.com
  - '*.kuwo.cn'
  - api-jooxtt.sanook.com
  - api.joox.com
  - joox.com
  - y.qq.com
  - '*.y.qq.com'
  - streamoc.music.tc.qq.com
  - mobileoc.music.tc.qq.com
  - isure.stream.qqmusic.qq.com
  - dl.stream.qqmusic.qq.com
  - aqqmusic.tc.qq.com
  - amobile.music.tc.qq.com
  - '*.xiami.com'
  - '*.music.migu.cn'
  - music.migu.cn
  - '*.msftconnecttest.com'
  - '*.msftncsi.com'
  - msftconnecttest.com
  - msftncsi.com
  - localhost.ptlogin2.qq.com
  - localhost.sec.qq.com
  - +.srv.nintendo.net
  - +.stun.playstation.net
  - xbox.*.microsoft.com
  - '*.*.xboxlive.com'
  - +.battlenet.com.cn
  - +.wotgame.cn
  - +.wggames.cn
  - +.wowsgame.cn
  - +.wargaming.net
  - proxy.golang.org
  - stun.*.*
  - stun.*.*.*
  - +.stun.*.*
  - +.stun.*.*.*
  - +.stun.*.*.*.*
  - heartbeat.belkin.com
  - '*.linksys.com'
  - '*.linksyssmartwifi.com'
  - '*.router.asus.com'
  - mesu.apple.com
  - swscan.apple.com
  - swquery.apple.com
  - swdownload.apple.com
  - swcdn.apple.com
  - swdist.apple.com
  - lens.l.google.com
  - stun.l.google.com
  - +.nflxvideo.net
  - '*.square-enix.com'
  - '*.finalfantasyxiv.com'
  - '*.ffxiv.com'
  - '*.mcdn.bilivideo.cn'
  - WORKGROUP
  default-nameserver:
  - 223.5.5.5
  - 119.29.29.29
  nameserver:
  - https://223.5.5.5/dns-query
  - https://223.6.6.6/dns-query
  - https://1.12.12.12/dns-query
  - https://120.53.53.53/dns-query
  nameserver-policy: null
  fallback: null
  fallback-filter: null
```
