---
description: Stripping Payload, VLAN, MPLS, Tunnel header, etc.
---

# Packet Stripping

### Stripping Payload

```xml
<run>
    <output id="1">
        <port>P7</out>
        <stripping>payload</stripping>
    </output>
    <chain>
        <in>P6</in>
        <out>O1</out>
    </chain>
</run>
```

### Stripping VLAN

```xml
<run>
    <output id="1">
        <port>P7</out>
        <stripping>vlan</stripping>
    </output>
    <chain>
        <in>P6</in>
        <out>O1</out>
    </chain>
</run>
```

### Stripping GTP header

```xml
<run>
    <output id="1">
        <port>P7</out>
        <stripping>gtp</stripping>
    </output>
    <chain>
        <in>P6</in>
        <out>O1</out>
    </chain>
</run>
```

### Striping MPLS

```xml
<run>
    <output id="1">
        <port>P7</out>
        <stripping>mpls</stripping>
    </output>
    <chain>
        <in>P6</in>
        <out>O1</out>
    </chain>
</run>
```
