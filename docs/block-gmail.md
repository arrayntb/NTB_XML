---
description: Block Gmail by block dns query mail.google.com and block tcp dest port 53
---

# Block Gmail

```
        WAN
         |
         |P7 
  --------------- 
 |               |
 |   Array NTB   |
 |               |
  --------------- 
         |P6
         |
        LAN
```

```xml
<run>
    <filter id="1" sessionBase="no">
        <or>
            <find name="dns.qry.name" relation="==" content="mail.google.com"/>
        </or>
    </filter>
     <filter id="2" sessionBase="no">
        <or>
            <find name="tcp.dstport" relation="==" content="53"/>
        </or>
    </filter>
    <chain>
        <in>P6</in>
        <fid type="or">F1,F2</fid>
        <out>0</out>
        <next type="notmatch">
            <out>P7</out>
        </next>
    </chain>
    <chain>
        <in>P7</in>
        <out>P6</out>
    </chain>
</run>
```
