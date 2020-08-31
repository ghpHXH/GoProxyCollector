我是光年实验室高级招聘经理。
我在github上访问了你的开源项目，你的代码超赞。你最近有没有在看工作机会，我们在招软件开发工程师，拉钩和BOSS等招聘网站也发布了相关岗位，有公司和职位的详细信息。
我们公司在杭州，业务主要做流量增长，是很多大型互联网公司的流量顾问。公司弹性工作制，福利齐全，发展潜力大，良好的办公环境和学习氛围。
公司官网是http://www.gnlab.com,公司地址是杭州市西湖区古墩路紫金广场B座，若你感兴趣，欢迎与我联系，
电话是0571-88839161，手机号：18668131388，微信号：echo 'bGhsaGxoMTEyNAo='|base64 -D ,静待佳音。如有打扰，还请见谅，祝生活愉快工作顺利。

# GoProxyCollector
## Introduction
GoProxyCollector is a lightweight, out-of-box proxy collector written in Go.
## Features
- Out-of-box: Use [boltdb](https://github.com/boltdb/bolt) as embedded storage. No need to install extra database.
- Configurable: Easy to support more proxy website by add your own rule in configuration.
## Getting Start
- Make sure your Go version >= 1.9
- Download
```
go get -u github.com/AceDarkknight/GoProxyCollector
```
- Start up
```
go run main.go
```
## Usage
- Get a ip:
```
GET http://localhost:8090/get
```
The response is json.
```json
{
   "ip":"1.180.235.165",
   "port":3128,
   "location":"内蒙古 电信",
   "source":"http://www.ip181.com/",
   "speed":0.13
}
```
- Delete a ip:
```
GET http://localhost:8090/delete?ip=1.2.3.4
```

##  Data Source
Currently this project will fetch proxy from these website.
- http://www.xicidaili.com
- http://www.89ip.cn
- http://www.kxdaili.com/
- https://www.kuaidaili.com
- http://www.ip3366.net/
- http://www.ip181.com/
- http://www.data5u.com
- https://proxy.coderbusy.com

## Extension
If you want to fetch from other website, you can modify the configuration file.
```
github.com\AceDarkknight\GoProxyCollector\collectorConfig.xml
```

Here is a standard config example:
```xml
<config name="xicidaili">
    <urlFormat>http://www.xicidaili.com/nn/%s</urlFormat>
    <urlParameters>1,2,3</urlParameters>
    <collectType>0</collectType>
    <charset>utf-8</charset>
    <valueNameRuleMap>
        <item name="table" rule="#ip_list tr:not(:first-child)"/>
        <item name="ip" rule="td:nth-child(2)"/>
        <item name="port" rule="td:nth-child(3)"/>
        <item name="location" rule="td:nth-child(4) a"/>
        <item name="speed" rule="td:nth-child(7) div" attribute="title"/>
    </valueNameRuleMap>
</config>
```
- **name**: this attribute represents the collector's name for debugging.
- **urlFormat** and **urlParameters**: to generate the target url. For the example, collector will collect from

    > http://www.xicidaili.com/nn/1

    > http://www.xicidaili.com/nn/2

    > http://www.xicidaili.com/nn/3

- **collectType**: 0 represents call selectorCollector and 1 represents to call regexCollector.
- **charset**: the charset of website. The default value is utf-8.
- **valueNameRuleMap**: the rule to collect item we need. See more detail from [goquery](https://github.com/PuerkitoBio/goquery) document.
