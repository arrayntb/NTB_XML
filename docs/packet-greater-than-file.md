---
description: Save packet to pcap
---

# Packet 2 pcap

## Basic

Save packet from P0 to H1/default&#x20;

```xml
<run>
    <output id="1">
        <port>H1</port>
        <dir>default</dir>
    </output>
    <chain>
        <in>P0</in>
        <out>O1</out>
    </chain>
</run>
```

## Save DNS packet to pcap

```xml
<run>
    <filter id="1" sessionBase="no">
        <or>
            <find name="udp.port" relation="==" content="53"/>
        </or>
    </filter>
    <output id="1">
        <port>H1</port>
        <dir category="day">dns</dir>
    </output>
    <chain>
        <in>P6</in>
        <fid>F1</fid>
        <out>O1</out>
    </chain>
</run>
```

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>
