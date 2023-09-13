# Bandwidth Control

Example: Limit only 192.168.1.0/24 bandwidth 1M-10M, others remain full line rate.

```markup
<run>
    <filter id="1" alt="ip bandwidth need control" sessionBase="no">
        <or>
            <find name="ip.addr" relation="==" content="192.168.1.0/24"/>
        </or>
    </filter>
    <output id="1" alt="limit 1M-10M" minbps="1000000" maxbps="10000000">
        <port>P7</port>
    </output>
    <chain>
        <in>P6</in>
        <fid>F1</fid>
        <out>O1</out>
        <next type="notmatch">
            <out>P7</out>
        </next>
    </chain>
</run>

```
