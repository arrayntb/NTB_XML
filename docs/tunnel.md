---
description: GRE/GTP/UDP Tunnel
---

# Tunnel

### GRE Tunnel

#### Config XML

```xml
<configSet reboot="no">
    <ifcfgs>
        <find role="ft">
            <enable>True</enable>
            <name>P7</name>
            <ip>192.168.2.10</ip>
            <netmask>255.255.255.0</netmask>
            <gateway></gateway>
        </find>
    </ifcfgs>
</configSet>
```

#### ARRAY NTB XML

```xml
<run>
    <output id="1">
        <port>P7</port>
        <nvgre_dip>192.168.2.201</nvgre_dip>
    </output>
    <chain>
        <in>P6</in>
        <out>O1</out>
    </chain>
</run>
```
