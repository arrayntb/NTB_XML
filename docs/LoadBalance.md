# Load Balance

### basic

P0,P1 load balance to P3,P4,P5,P6 by 5-tuple (sip,dip,sport,dport,ip\_proto)

```xml
<run>
    <chain>
        <in>P0,P1</in>
        <out type="loadBalance" lbtype="5thash">P3,P4,P5,P6</out>
    </chain>
</run>
```

or

```javascript
<run>
    <script src="common.js"></script>
    <script>
    <![CDATA[
        port_loadbalance ('P0,P1', 'P3,P4,P5,P6');
    ]]>
    </script>
</run>
```

### inline loadbalance to IPS1 and IPS2

```
                         WAN
                          |
                          |P7 
                    --------------- 
  ---------        |               |
 |         |----P5 |               |
 |   IPS1  |       |               |
 |         |----P4 |               |
  ---------        |      NTB      |
  ---------        |               |
 |         |----P3 |               |
 |   IPS2  |       |               |
 |         |----P2 |               |
  ---------        |               |
                    --------------- 
                           |P6
                           |
                          LAN
```

```xml
<run>
    <chain>
        <in>P6</in>
        <out type="loadBalance" lbtype="5thash">P2,P4</out>
    </chain>
    <chain>
        <in>P7</in>
        <out type="loadBalance" lbtype="5thash">P3,P5</out>
    </chain>
    <chain>
         <in>P2,P4</in>
         <out>P6</out>
    </chain>
    <chain>
         <in>P3,P5</in>
         <out>P7</out>
    </chain>
</run>
```
