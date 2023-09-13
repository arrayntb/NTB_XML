# Inline & Bypass

### basic inline

```
        WAN
         |
         |P7 
  --------------- 
 |               |
 |               |
 |      NTB      |
 |               |
 |               |
  --------------- 
         |P6
         |
        LAN
```

```xml
<run>
    <chain>
        <in>P6</in>
        <out>P7</out>
    </chain>
    <chain>
        <in>P7</in>
        <out>P6</out>
    </chain>
</run>
```

or

```javascript
<run>
    <script src="common.js"></script>
    <script>
    <![CDATA[
        port_inline ('P6', 'P7');
    ]]>
    </script>
</run>
```

### basic inline & bypass with heartbeat

```
                         WAN
                          |
                          |P7 
                    --------------- 
  ---------        |               |
 |         |----P5 |               |
 |   IPS   |       |      NTB      |
 |         |----P4 |               |
  ---------        |               |
                    --------------- 
                           |P6
                           |
                          LAN
```

**Config XML**

Send heartbeat packet to P4 and expect packet will come back from P5

```xml
<configSet reboot="no">
    <heartbeat>
        <enable>True</enable>
        <frequency>500</frequency>
        <maxAllowTimeouts>3</maxAllowTimeouts>
        <target>
            <enable>True</enable>
            <sendPort>P4</sendPort>
            <receivePort>P5</receivePort>
            <packetData>000d48285134000d482851338137ffff0030000000004004eca2c6130102c61301010000000000000000000000000000000000000000000000000000</packetData>
            <description>IPS</description>
            <id>1</id>
        </target>
    </heartbeat>
</configSet>
```

**ARRAY XML**

```xml
<run>
    <filter id="1" sessionBase="no">
        <or>
            <find name="heartbeat.target.miss.id" relation="==" content="1"/>
        </or>
    </filter>
    <chain>
        <in>P6</in>
        <fid>F1</fid>
        <out>P7</out>
        <next type="notmatch">
            <out>P4</out>
        </next>
    </chain>
    <chain>
        <in>P7</in>
        <fid>F1</fid>
        <out>P6</out>
        <next type="notmatch">
            <out>P5</out>
        </next>
    </chain>
    <chain>
        <in>P4</in>
        <out>P6</out>
    </chain>
    <chain>
        <in>P5</in>
        <out>P7</out>
    </chain>
</run>
```

or

```javascript
<run>
    <script src="common.js"></script>
    <script>
    <![CDATA[
        port_inline_bypass ('P4','P5','P6','P7');
    ]]>
    </script>
</run>
```

### inline block HTTP url and HTTPS server name

```
        WAN
         |
         |P7 
  --------------- 
 |               |
 |               |
 |      NTB      |
 |               |
 |               |
  --------------- 
         |P6
         |
        LAN
```

```xml
<run>
	<filter id="1" sessionBase="no">
		<or>
			<find name="ssl.server_name" relation="==" content="googlevideo.com"/>
		</or>
	</filter>
	<filter id="2" sessionBase="no">
		<or>
			<find name="ssl.server_name_public_suffix" relation="==" content="*.googlevideo.com" />
		</or>
	</filter>
	<filter id="3" sessionBase="no">
		<or>
			<find name="http.request.url" relation="==" content="yahoo.com/index.html"/>
		</or>
	</filter>
	<chain>
		<in>P6</in>
		<fid type="or">F1,F2,F3</fid>
		<out>0</out>
		<next type="notmatch">
			<out>P7</out>
		</next>
	</chain>
	<chain>
		<in>P7</in>
		<fid type="or">F1,F2,F3</fid>
		<out>0</out>
		<next type="notmatch">
			<out>P6</out>
		</next>
	</chain>
</run>
```

### inline block HTTP url and HTTPS server name (regex)

```
        WAN
         |
         |P7 
  --------------- 
 |               |
 |               |
 |      NTB      |
 |               |
 |               |
  --------------- 
         |P6
         |
        LAN
```

```xml
<run>
        <filter id="1" sessionBase="no">
                <or>
                        <find name="ssl.handshake.type" relation="==" content="0" />
                        <find name="ssl.handshake.type" relation="==" content="1" />
                </or>
        </filter>
        <filter id="2" sessionBase="no">
                <or>
                        <find name="regex" relation="==" content="{s}googlevideo\.com"/>
                </or>
        </filter>
        <filter id="3" sessionBase="no">
                <or>
                        <find name="http.request.url" relation="==" content="yahoo.com/index.html"/>
                </or>
        </filter>
        <chain>
                <in>P6</in>
                <fid type="and">F1,F2</fid>
                <out>0</out>
                <next type="notmatch">
                        <fid>F3</fid>
                        <out>0</out>
                        <next type="notmatch">
                                <out>P7</out>
                        </next>
                </next>
        </chain>
        <chain>
                <in>P7</in>
                <fid type="and">F1,F2</fid>
                <out>0</out>
                <next type="notmatch">
                        <fid>F3</fid>
                        <out>0</out>
                        <next type="notmatch">
                                <out>P6</out>
                        </next>
                </next>
        </chain>
</run>
```

### inline bypass 443 without first 10 packets

```
                         WAN
                          |
                          |P7 
                    --------------- 
  ---------        |               |
 |         |----P5 |               |
 |   IPS   |       |      NTB      |
 |         |----P4 |               |
  ---------        |               |
                    --------------- 
                           |P6
                           |
                          LAN
```

```xml
<run>
    <filter id="1" sessionBase="no">
        <or>
            <find name="tcp.port" relation="==" content="443" />
            <find name="udp.port" relation="==" content="443" />
        </or>
    </filter>
    <filter id="2" sessionBase="no" maxPackets="10">
        <or>
            <find name="tcp.port" relation="==" content="443" />
            <find name="udp.port" relation="==" content="443" />
        </or>
    </filter>
    <chain>
        <in>P6</in>
        <fid>F1</fid>
        <next>
            <fid>F2</fid>
            <out>P4</out>
            <next type="notmatch">
                <out>P7</out>
            </next>
        </next>
        <next type="notmatch">
            <out>P4</out>
        </next>
    </chain>
    <chain>
        <in>P7</in>
        <fid>F1</fid>
        <next>
            <fid>F2</fid>
            <out>P5</out>
            <next type="notmatch">
                <out>P6</out>
            </next>
        </next>
        <next type="notmatch">
            <out>P5</out>
        </next>
    </chain>
    <chain>
        <in>P4</in>
        <out>P6</out>
    </chain>
    <chain>
        <in>P5</in>
        <out>P7</out>
    </chain>
</run>
```

### inline bypass HTTPS Youtube

```
                         WAN
                          |
                          |P7 
                    --------------- 
  ---------        |               |
 |         |----P5 |               |
 |   IPS   |       |      NTB      |
 |         |----P4 |               |
  ---------        |               |
                    --------------- 
                           |P6
                           |
                          LAN
```

```xml
<run>
    <filter id="5" sessionBase="yes">
        <or>
            <find name="ssl.server_name" relation="==" content="youtube.com"/>
            <find name="ssl.server_name" relation="==" content="youtu.be"/>
            <find name="ssl.server_name" relation="==" content="yt3.ggpht.com"/>
        </or>
    </filter>
    <filter id="6" sessionBase="yes">
        <or>
            <find name="ssl.server_name_public_suffix" relation="==" content="*.googlevideo.com" />
            <find name="ssl.server_name_public_suffix" relation="==" content="*.googleapis.com" />
            <find name="ssl.server_name_public_suffix" relation="==" content="*.youtube.com" />
            <find name="ssl.server_name_public_suffix" relation="==" content="*.ytimg.com" />
        </or>
    </filter>
    <filter id="7" sessionBase="yes">
        <or>
            <find name="flowtable.matched.fid" relation="==" content="F5" />
            <find name="flowtable.matched.fid" relation="==" content="F6" />
        </or>
    </filter>
    <chain>
        <in>P6</in>
        <fid>F5,F6</fid>
        <out>P7</out>
        <next type="notmatch">
            <out>P4</out>
        </next>
    </chain>
    <chain>
        <in>P7</in>
        <fid>F7</fid>
        <out>P6</out>
        <next type="notmatch">
            <out>P5</out>
        </next>
    </chain>
    <chain>
        <in>P4</in>
        <out>P6</out>
    </chain>
    <chain>
        <in>P5</in>
        <out>P7</out>
    </chain>
</run>
```

### inline 2 in 2 out and bypass SSL

```
                    --------------- 
  ---------        |               |
 |         |----P5 |               | P7 ---- WAN1
 |   IPS1  |       |               |
 |         |----P4 |               | P6 ---- LAN1
  ---------        |      NTB      |
  ---------        |               |
 |         |----P3 |               | P1 ---- WAN2
 |   IPS2  |       |               |
 |         |----P2 |               | P0 ---- LAN2
  ---------        |               |
                    --------------- 
```

```xml
<run>
    <filter id="1" name="1" sessionBase="yes">
        <or>
            <find name="ssl.handshake.type" relation="==" content="0" />
            <find name="ssl.handshake.type" relation="==" content="1" />
        </or>
    </filter>
    <filter id="2" sessionBase="yes">
        <or>
            <find name="flowtable.matched.fid" relation="==" content="F1" />
        </or>
    </filter>
    <chain sessionUnique="yes">
        <in>P0</in>
        <fid>F1</fid>
        <out>P1</out>
        <next type="notmatch">
            <out>P2</out>
        </next>
    </chain>
    <chain sessionUnique="yes">
        <in>P1</in>
        <fid>F2</fid>
        <out>P0</out>
        <next type="notmatch">
            <out>P3</out>
        </next>
    </chain>
    <chain>
        <in>P2</in>
        <out>P0</out>
    </chain>
    <chain>
        <in>P3</in>
        <out>P1</out>
    </chain>
    <chain sessionUnique="yes">
        <in>P6</in>
        <fid>F1</fid>
        <out>P7</out>
        <next type="notmatch">
            <out>P4</out>
        </next>
    </chain>
    <chain sessionUnique="yes">
        <in>P7</in>
        <fid>F2</fid>
        <out>P6</out>
        <next type="notmatch">
            <out>P5</out>
        </next>
    </chain>
    <chain>
        <in>P4</in>
        <out>P6</out>
    </chain>
    <chain>
        <in>P5</in>
        <out>P7</out>
    </chain>
</run>
```

### inline 2 LAN to 1 WAN

```
        WAN
         |
         |P7 
  --------------- 
 |               |
 |               |
 |      NTB      |
 |               |
 |               |
  --------------- 
     |P5    |P6
     |      |
    LAN1   LAN2
```

```xml
<run>
    <filter id="1" name="1" sessionBase="yes">
          <or>
            <find name="flowtable.inport" relation="==" content="P5" />
        </or>
    </filter>
    <filter id="2" sessionBase="yes">
        <or>
            <find name="flowtable.inport" relation="==" content="P6" />
        </or>
    </filter>
    <chain>
        <in>P5,P6</in>
        <out>P7</out>
    </chain>
    <chain>
        <in>P7</in>
        <fid>F1</fid>
        <out>P5</out>
        <next type="notmatch">
            <fid>F2</fid>
            <out>P6</out>
        </next>
    </chain>
</run>
```
