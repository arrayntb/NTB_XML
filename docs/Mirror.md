---
description: Duplicate packets. one to one, one to many, many to one, many to many
---

# Mirror

### Packets from P0 duplicate to P1,P2,P3,P4

```xml
<run>
    <chain>
        <in>P0</in>
        <out>P1,P2,P3,P4</out>
    </chain>
</run>
```

or

```javascript
<run>
    <script src="common.js"></script>
    <script>
    <![CDATA[
        port_mirror('P0', 'P1,P2,P3,P4');
    ]]>
    </script>
</run>
```
