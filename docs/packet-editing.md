---
description: Edit packet header like Mac address, IP address and packet payload masking
---

# Packet Editing

### Modify Mac address

```xml
<run>
    <output id="1">
        <port>P7</port>
        <modify_dstmac>d8:fe:e3:a4:d3:78</modify_dstmac>
    </output>
    <chain>
        <in>P6</in>
        <out>O1</out>
    </chain>
</run>
```

### Modify IP address

```xml
<run>
    <filter id="1" sessionBase="no">
    <or>
	<find name="udp.dstport" relation="==" content="53" />
    </or>
    </filter>
    <output id="1">
        <port>P7</port>
        <modify_dstip>8.8.8.8</modify_srcip>
    </output>
    <chain>
        <in>P6</in>
        <fid>F1</fid>
        <out>O1</out>
    </chain>
</run>
```

### Payload Masking

```xml
<run>
    <filter id="1" sessionBase="no" masking="yes">
    <or>
        <find name="regex" relation="==" content="{t}[a-zA-Z]\d{9}"></find>
    </or>
    </filter>
    <chain>
        <in>P6</in>
        <fid>F1</fid>
        <out>P7</out>
    </chain>
</run>
```
