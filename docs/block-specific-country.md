---
description: block specific country try to establish TCP/UDP connection from WAN to LAN
---

# Block Specific Country

```
        WAN
         |
         |P7 
  --------------- 
 |               |
 |      NTB      |
 |               |
  --------------- 
         |P6
         |
        LAN
```

```xml
<run>
    <filter id="778">
        <or>
            <find name="country.iso_code" relation="==" content="CN"/>
            <find name="country.iso_code" relation="==" content="RU"/>
        </or>
    </filter>
    <filter id="1" sessionBase="no">
        <and>
            <find name="tcp.flags.syn" relation="==" content="1"/>
            <find name="tcp.flags.ack" relation="==" content="0"/>
        </and>
    </filter>
    <filter id="2" sessionBase="no">
        <and>
            <find name="udp" relation="==" content=""/>
            <find name="session.packet.nth" relation="==" content="1"/>
        </and>
    </filter>
    <chain>
        <in>P6</in>
        <out>P7</out>
    </chain>
    <chain>
        <in>P7</in>
        <fid>F1,F2</fid>
        <next>
            <fid>F778</fid>
            <out>0</out>
            <next type="notmatch">
                <out>P6</out>
            </next>
        </next>
        <next type="notmatch">
            <out>P6</out>
        </next>
    </chain>
</run>
```
