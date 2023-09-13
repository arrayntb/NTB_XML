---
description: >-
  Network Address Translation(NAT), our implement is more like Port Address
  Translation(PAT), but I think call it Flow Address Translation(FAT) might be
  better.
---

# L3 Switch NAT breakout



* P6: inner
* P7: outer
* P5: other outer

## Config XML

```xml
<configSet reboot="no">
    <args>
        <nat>true</nat>   
    </args>
</configSet>
```

## ARRAY XML

### breakout dns (basic sample)

```markup
<run>
    <filter id="99" alt="dns query" sessionBase="no">
        <or>
            <find name="udp.port" relation="==" content="53"/>
        </or>
    </filter>
    <filter id="3" sessionBase="no">
        <or>
            <find name="arp.request.target.ip" relation="==" content="172.16.10.10"/>
        </or>
    </filter>
    <output id="3">
        <port>P5</port>
        <arp_reply_default_mac/>
    </output>
    <output id="5">
        <port>P5</port>
        <modify_src_default_mac/>
        <modify_srcip nat="yes">172.16.10.10</modify_srcip>
        <gateway>172.16.10.1</gateway>
    </output>
    <output id="6" arp_dstip_mac="yes">
        <port>P6</port>
    </output>
    <chain>
        <in>P6</in>
        <fid type="and">F99</fid>
        <out>O5</out>
        <next type="notmatch">
            <out>P7</out>
        </next>
    </chain>
    <chain>
        <in>P7</in>
        <out>P6</out>
    </chain>
    <chain>
        <in>P5</in>
        <fid>F3</fid>
        <out>O3</out>
        <next type="notmatch">
            <out>O6</out>
        </next>
    </chain>
</run>
```

### breakout dns and replace dns query server

* Set P6 NAT Breakout (only dns query to 8.8.8.8) to P5
* modify dns server from 8.8.8.8 to 168.95.1.1

```xml
<run>
    <filter id="99" alt="dns query" sessionBase="no">
        <or>
            <find name="udp.port" relation="==" content="53"/>
        </or>
    </filter>
    <filter id="100" alt="breakout server" sessionBase="no">
        <or>
            <find name="ip.dst" relation="==" content="8.8.8.8"/>
        </or>
    </filter>
    <filter id="101" alt="dns from server" sessionBase="no">
        <or>
            <find name="ip.src" relation="==" content="168.95.1.1"/>
        </or>
    </filter>
    <filter id="3" sessionBase="no">
        <or>
            <find name="arp.request.target.ip" relation="==" content="172.16.10.10"/>
        </or>
    </filter>
    <output id="3">
        <port>P5</port>
        <arp_reply_default_mac/>
    </output>
    <output id="5">
        <port>P5</port>
        <modify_src_default_mac/>
        <modify_dstip>168.95.1.1</modify_dstip>
        <modify_srcip nat="yes">172.16.10.10</modify_srcip>
        <gateway>172.16.10.1</gateway>
    </output>
    <output id="6" arp_dstip_mac="yes">
        <port>P6</port>
    </output>
    <output id="101" arp_dstip_mac="yes">
        <port>P6</port>
        <modify_srcip>8.8.8.8</modify_srcip>
    </output>
    <chain>
        <in>P6</in>
        <fid type="and">F99,F100</fid>
        <out>O5</out>
        <next type="notmatch">
            <out>P7</out>
        </next>
    </chain>
    <chain>
        <in>P7</in>
        <out>P6</out>
    </chain>
    <chain>
        <in>P5</in>
        <fid>F3</fid>
        <out>O3</out>
        <next type="notmatch">
            <fid>F101</fid>
            <out>O101</out>
            <next type="notmatch">
                <out>O6</out>
            </next>
        </next>
    </chain>
</run>
```

### breakout ssh and reply ICMP fragmentation needed if packet length over 1500

```markup
<run>
    <filter id="99" alt="ssh" sessionBase="no">
        <or>
            <find name="tcp.port" relation="==" content="22"/>
        </or>
    </filter>
    <filter id="3" sessionBase="no">
        <or>
            <find name="arp.request.target.ip" relation="==" content="172.16.10.10"/>
        </or>
    </filter>
    <filter id="4" alt="df and packet over 1500" sessionBase="no">
        <and>
            <find name="ip.flags.df" relation="==" content="1"/>
            <find name="packet.len" relation="&gt;=" content="1500"/>
        </and>
    </filter>
    <output id="3">
        <port>P5</port>
        <arp_reply_default_mac/>
    </output>
    <output id="5">
        <port>P5</port>
        <modify_src_default_mac/>
        <modify_srcip nat="yes">172.16.10.10</modify_srcip>
        <gateway>172.16.10.1</gateway>
    </output>
    <output id="6" arp_dstip_mac="yes">
        <port>P6</port>
    </output>
    <output id="7">
        <port>P6</port>
        <icmp_reply_fragment_need mtu="1440"/>
        <modify_srcip>172.16.10.10</modify_srcip>
    </output>
    <chain>
        <in>P6</in>
        <fid>F4</fid>
        <out>O7</out>
        <next type="notmatch">
            <fid type="and">F99</fid>
            <out>O5</out>
            <next type="notmatch">
                <out>P7</out>
            </next>
        </next>
    </chain>
    <chain>
        <in>P7</in>
        <out>P6</out>
    </chain>
    <chain>
        <in>P5</in>
        <fid>F3</fid>
        <out>O3</out>
        <next type="notmatch">
            <out>O6</out>
        </next>
    </chain>
</run>
```
