---
description: concat all L2 Switches with GRE tunnel
---

# L2 GRE Breakout

```
                                     
                                            ---------------
                                           | GRE(source)   |    ---------
                    -------------       ---| 192.168.2.100 |---|L2 Switch|--- user
    ---------      |             |     |    ---------------     ---------
   |L2 Switch|---P0|    GRISM    |P1---|    
    ---------      | GRE(dest)   |     |    ---------------     ---------
        |          | 192.168.2.1 |      ---| GRE(source)   |---|L2 Switch|--- user
       user         -------------      |   | 192.168.2.101 |    ---------
                                       |    ---------------
                                       |
                                        ---... more   
```

## Config XML

```xml
<configSet reboot="no">
    <args>
        <grel2Correlation>True</grel2Correlation>
        <grel2CorrelationPort>P1</grel2CorrelationPort>   
    </args>
    <filters>
        <in-tunnels>     
            <GRE>True</GRE>
        </in-tunnels>
    </filters>
</configSet>
```

## GRISM XML

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
        <stripping>gre</stripping>
    </output>
    <output id="3">
        <port>P1</port>
        <tagging>l2gre</tagging>
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
