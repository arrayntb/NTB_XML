---
description: packet tagging VLAN/Timestamp
---

# Packet Tagging

### VLAN Tagging

```xml
<run>
  <output id="1">
    <port>P7</port>
    <Q>10</Q>
  </output>
  <chain>
    <in>P6</in>
    <out>O1</out>
  </chain>
</run>
```

### Timestamp Tagging

```xml
<run>
  <output id="1">
    <port>P7</port>
    <tagging>timestamp</tagging>
  </output>
  <chain>
    <in>P6</in>
    <out>O1</out>
  </chain>
</run>
```
