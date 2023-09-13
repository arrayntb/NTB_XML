# L2 Switch Like

set P4,P5,P6,P7 as L2 Switch

## Config XML

```xml
<configSet reboot="no">
    <args>
        <macTable>P4,P5,P6,P7</macTable>   
    </args>
</configSet>
```

## ARRAY XML

```xml
<run>
    <filter id="100" alt="broadcast" sessionBase="no">
        <or>
            <find name="eth.dst" relation="==" content="FF:FF:FF:FF:FF:FF"/>
        </or>
    </filter>
    <chain>
        <in>P4</in>
        <fid>F100</fid>
        <out>P5,P6,P7</out>
        <next type="notmatch">
            <out>S</out>
        </next>
    </chain>
    <chain>
        <in>P5</in>
        <fid>F100</fid>
        <out>P4,P6,P7</out>
        <next type="notmatch">
            <out>S</out>
        </next>
    </chain>
    <chain>
        <in>P6</in>
        <fid>F100</fid>
        <out>P4,P5,P7</out>
        <next type="notmatch">
            <out>S</out>
        </next>
    </chain>
    <chain>
        <in>P7</in>
        <fid>F100</fid>
        <out>P4,P5,P6</out>
        <next type="notmatch">
            <out>S</out>
        </next>
    </chain>
</run>
```
