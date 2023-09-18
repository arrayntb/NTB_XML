---
description: Translate Snort Rule to Array NTB XML
---

# Snort Rule > Array NTB XML



## HOME NET

```
ipvar $HOME_NET 10.0.2.0/24
```

```xml
<filter id="1" sessionBase="no" alt="HOME_NET">
    <or>
        <find name="ip.src" relation="==" content="10.0.2.0/24"/>
    </or>
</filter>
```

## EXTERNAL NET

```
ipvar $EXTERNAL_NET any
```

```xml
<filter id="2" sessionBase="no" alt="EXTERNAL_NET">
    <or>
    </or>
</filter>
```

## HTTP PORTS

```
portvar MY_HTTP_DST_PORTS [80,8080]
```

```xml
<filter id="3" sessionBase="no" alt="MY_HTTP_DST_PORTS">
    <or>
        <find name="tcp.dstport" relation="==" content="80"/>
        <find name="tcp.dstport" relation="==" content="8080"/>
    </or>
</filter>
```

## Rule1

```
alert tcp $HOME_NET any -> $EXTERNAL_NET $HTTP_PORTS
(
  msg:"Rule1";
  flow:stateless;
  http_uri;
  content:"/vi/push";
  http_header;
  content:"Accept:*/*";
  content:"Accept-Encoding: gzip, deflate, br";
  content:"Accept-Language: en-US|0D 0A|";
  content:"{|22|locale|22|:|22|en|22|,|22|channel|22|:|22|prod|22|,|22|addon|22|:|22|",fast_pattern,nocase;
  content:"cli";
)
```

```xml
<filter id="101" sessionBase="no" alt="rule1">
    <and>
        <find name="ip.proto" relation="==" content="6"/>
        <find name="http.request.uri" relation="==" content="/v1/push"/>
        <find name="regex" relation="==" content="Accept: */*"/>
        <find name="regex" relation="==" content="Accept-Encoding: gzip, deflate, br"/>
        <find name="regex" relation="==" content="Accept-Language: en-US|0D 0A|"/>
        <find name="regex" relation="==" content="{i}\{|22|locale|22|:|22|en|22|,|22|channel|22|:|22|prod|22|,|22|addon|22|:|22|"/>
        <find name="regex" relation="==" content="cli"/>
    </and>
</filter>
<chain>
    <in>P0</in>
    <fid type="and">F1,F2,F3,F101</fid>
    <out>P1</out>
</chain>

```
