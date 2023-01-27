## 1.点击“设置-路由设置-启用高级路由-高级功能-添加规则集”，并输入名称；  
## 2.双击添加好的规则集，点击“导入规则-从剪切板粘贴”
规则如下：
GFWlist黑名单（PAC代理模式）  
```
[
  {
    "outboundTag": "proxy",
    "domain": [
      "#以下三行是GitHub网站，为了不影响下载速度走代理",
      "github.com",
      "githubassets.com",
      "githubusercontent.com"
    ]
  },
  {
    "outboundTag": "block",
    "domain": [
      "#阻止CrxMouse鼠标手势收集上网数据",
      "mousegesturesapi.com"
    ]
  },
  {
    "outboundTag": "direct",
    "domain": [
      "bitwarden.com",
      "bitwarden.net",
      "baiyunju.cc",
      "letsencrypt.org",
      "adblockplus.org",
      "safesugar.net",
      "#下两行谷歌广告",
      "googleads.g.doubleclick.net",
      "adservice.google.com",
      "#【以下全部是geo预定义域名列表】",
      "#下一行是所有私有域名",
      "geosite:private",
      "#下一行包含常见大陆站点域名和CNNIC管理的大陆域名，即geolocation-cn和tld-cn的合集",
      "geosite:cn",
      "#下一行包含所有Adobe旗下域名",
      "geosite:adobe",
      "#下一行包含所有Adobe正版激活域名",
      "geosite:adobe-activation",
      "#下一行包含所有微软旗下域名",
      "geosite:microsoft",
      "#下一行包含微软msn相关域名少数与上一行微软列表重复",
      "geosite:msn",
      "#下一行包含所有苹果旗下域名",
      "geosite:apple",
      "#下一行包含所有广告平台、提供商域名",
      "geosite:category-ads-all",
      "#下一行包含可直连访问谷歌网址，需要替换为加强版GEO文件，如已手动更新为加强版GEO文件，删除此行前面的#号使其生效",
      "#geosite:google-cn",
      "#下一行包含可直连访问苹果网址，需要替换为加强版GEO文件，如已手动更新为加强版GEO文件，删除此行前面的#号使其生效",
      "#geosite:apple-cn"
    ]
  },
  {
    "type": "field",
    "outboundTag": "proxy",
    "domain": [
      "#GFW域名列表",
      "geosite:gfw",
      "geosite:greatfire"
    ]
  },
  {
    "type": "field",
    "port": "0-65535",
    "outboundTag": "direct"
  }
]
```

Whitelist白名单（绕过大陆模式）  
```
[
  {
    "port": "",
    "outboundTag": "proxy",
    "ip": [],
    "domain": [
      "#以下三行是GitHub网站，为了不影响下载速度走代理",
      "github.com",
      "githubassets.com",
      "githubusercontent.com"
    ],
    "protocol": []
  },
  {
    "type": "field",
    "outboundTag": "block",
    "domain": [
      "#阻止CrxMouse鼠标手势收集上网数据",
      "mousegesturesapi.com",
      "#下一行广告管理平台网址，在ProductivityTab（原iChrome）浏览器插件页面显示",
      "cf-se.com"
    ]
  },
  {
    "type": "field",
    "port": "",
    "outboundTag": "direct",
    "ip": [
      "geoip:private",
      "geoip:cn"
    ],
    "domain": [
      "bitwarden.com",
      "bitwarden.net",
      "gravatar.com",
      "gstatic.com",
      "baiyunju.cc",
      "letsencrypt.org",
      "adblockplus.org",
      "safesugar.net",
      "#下两行谷歌广告",
      "googleads.g.doubleclick.net",
      "adservice.google.com",
      "#【以下全部是geo预定义域名列表】",
      "#下一行包含所有私有域名",
      "geosite:private",
      "#下一行包含常见大陆站点域名和CNNIC管理的大陆域名，即geolocation-cn和tld-cn的合集",
      "geosite:cn",
      "#下一行包含所有Adobe旗下域名",
      "geosite:adobe",
      "#下一行包含所有Adobe正版激活域名",
      "geosite:adobe-activation",
      "#下一行包含所有微软旗下域名",
      "geosite:microsoft",
      "#下一行包含微软msn相关域名少数与上一行微软列表重复",
      "geosite:msn",
      "#下一行包含所有苹果旗下域名",
      "geosite:apple",
      "#下一行包含所有广告平台、提供商域名",
      "geosite:category-ads-all",
      "#下一行包含可直连访问谷歌网址，需要替换为加强版GEO文件，如已手动更新为加强版GEO文件，删除此行前面的#号使其生效",
      "#geosite:google-cn",
      "#下一行包含可直连访问苹果网址，需要替换为加强版GEO文件，如已手动更新为加强版GEO文件，删除此行前面的#号使其生效",
      "#geosite:apple-cn"
    ],
    "protocol": []
  },
  {
    "type": "field",
    "port": "0-65535",
    "outboundTag": "proxy"
  }
]
```

全局代理
```
[
  {
    "port": "",
    "outboundTag": "block",
    "ip": [],
    "domain": [
      "#阻止CrxMouse鼠标手势收集上网数据",
      "mousegesturesapi.com",
      "#下一行广告管理平台网址，在ProductivityTab（原iChrome）浏览器插件页面显示",
      "cf-se.com"
    ],
    "protocol": []
  },
  {
    "type": "field",
    "port": "0-65535",
    "outboundTag": "proxy"
  }
]
```

全局直连
```
[
  {
    "port": "",
    "outboundTag": "block",
    "ip": [],
    "domain": [
      "#阻止CrxMouse鼠标手势收集上网数据",
      "mousegesturesapi.com",
      "#下一行广告管理平台网址，在ProductivityTab（原iChrome）浏览器插件页面显示",
      "cf-se.com"
    ],
    "protocol": []
  },
  {
    "port": "",
    "outboundTag": "proxy",
    "ip": [],
    "domain": [
      "#下一行ProductivityTab（原iChrome）浏览器插件",
      "ichro.me"
    ],
    "protocol": []
  },
  {
    "type": "field",
    "port": "0-65535",
    "outboundTag": "direct"
  }
]
```
