---
description: concat all L2 Switches with VXLAN tunnel
---

# VXLAN Breakout

```
                                     
                                            ---------------
                                           | VXLAN(source) |    ---------
                    -------------       ---| 192.168.2.100 |---|L2 Switch|--- user
    ---------      |             |     |    ---------------     ---------
   |L2 Switch|---P0|     NTB     |P1---|    
    ---------      | VXLAN(dest) |     |    
        |          | 192.168.2.1 |     |    ---------------     ---------
                   | :4789       |      ---| VXLAN(source) |---|L2 Switch|--- user
       user         -------------      |   | 192.168.2.101 |    ---------
                                       |    ---------------
                                       |
                                        ---... more   
```

## Config XML

```xml
<configSet reboot="no">
    <args>
        <vxlanCorrelation>True</vxlanCorrelation>
        <vxlanCorrelationPort>P1</vxlanCorrelationPort>   
    </args>
    <filters>
        <in-tunnels>     
            <VXLAN>True</VXLAN>
        </in-tunnels>
    </filters>
</configSet>
```

## ARRAY NTB XML

```xml
<run>
    <filter id="1" sessionBase="no">
        <or>
            <find name="arp.request.target.ip" relation="==" content="192.168.2.1"/>
        </or>
    </filter>
    <output id="1">
        <port>P1</port>
        <arp_reply_default_mac/>
    </output>
    <output id="2">
        <port>P0</port>
        <stripping>vxlan</stripping>
    </output>
    <output id="3">
        <port>P1</port>
        <tagging>vxlan</tagging>
    </output>
    <chain>
        <in>P1</in>
        <fid>F1</fid>
        <out>O1</out>
        <next type="notmatch">
            <out>O2</out>
        </next>
    </chain>
    <chain>
        <in>P0</in>
        <out>O3</out>
    </chain>
</run>
```
