# Mobile Edge Computing Breakout

4G/5G Mobile Edge Computing Breakout Sample

P5: Edge P6: eNB P7: Core

Functions

1. keep inline connect between Core and eNB
2. Set End User IP which need to be breakout to Edge Server
3. stripping GTP header before packet send to Edge
4. tagging GTP headaer when packet comes from Edge
5. reply ARP when Edge request for End User IP

```xml
<run>
    <filter id="1" sessionBase="no">
        <or>
            <find name="ip.src" relation="==" content="192.168.1.10"/>
            <find name="ip.src" relation="==" content="192.168.1.11"/>
        </or>
    </filter>
    <filter id="2" sessionBase="no">
        <or>
            <find name="arp.request.target.ip" relation="==" content="192.168.1.0/24"/>
        </or>
    </filter>
    <filter id="3" sessionBase="no">
        <or>
            <find name="ip.dst" relation="==" content="10.0.0.1"/>
            <find name="ip.dst" relation="==" content="10.0.0.2"/>
        </or>
    </filter>
    <output id="1" arp_dstip_mac="yes">
        <port>P5</port>
        <stripping>gtp</stripping>
    </output>
    <output id="2">
        <port>P6</port>
        <tagging>gtp2</tagging>
    </output>
    <output id="3">
        <port>P5</port>
        <arp_reply_default_mac/>
    </output>
    <chain>
        <in>P7</in>
        <out>P6</out>
    </chain>
    <chain>
        <in>P6</in>
        <fid type="and">F1,F3</fid>
        <out>O1</out>
        <next type="notmatch">
            <out>P7</out>
        </next>
    </chain>
    <chain>
        <in>P5</in>
        <fid>F2</fid>
        <out>O3</out>
        <next type="notmatch">
            <out>O2</out>
        </next>
    </chain>
</run>
```
