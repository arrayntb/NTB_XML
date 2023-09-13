## Service Chain with SSLi(Array) and IPS
```
                         WAN
                          |
                          |P7 
                    ---------------         ----------
                   |               | P1----|          |
  ---------        |               |       |   Array  |
 |         |----P5 |               | P3----|          |
 |   IPS   |       |   Array NTB   |        ----------
 |         |----P4 |               |        ----------
  ---------        |               | P2----|          |
                   |               |       |   Array  |
                   |               | P0----|          |
                    ---------------         ----------
                           |P6
                           |
                          LAN
```                     
#### Config XML
```xml
<configSet reboot="no">
    <heartbeat>
        <enable>True</enable>
        <frequency>500</frequency>
        <maxAllowTimeouts>3</maxAllowTimeouts>
        <target>
            <enable>True</enable>
            <sendPort>P0</sendPort>
            <receivePort>P2</receivePort>
            <packetData>000d48285134000d482851338137ffff0030000000004004eca2c6130102c61301010000000000000000000000000000000000000000000000000000</packetData>
            <description>Array1</description>
            <id>1</id>
        </target>
	    <target>
            <enable>True</enable>
            <sendPort>P1</sendPort>
            <receivePort>P3</receivePort>
            <packetData>000d48285134000d482851338137ffff0030000000004004eca2c6130102c61301010000000000000000000000000000000000000000000000000000</packetData>
            <description>Array2</description>
            <id>2</id>
        </target>
	    <target>
            <enable>True</enable>
            <sendPort>P4</sendPort>
            <receivePort>P5</receivePort>
            <packetData>000d48285134000d482851338137ffff0030000000004004eca2c6130102c61301010000000000000000000000000000000000000000000000000000</packetData>
            <description>IPS</description>
            <id>3</id>
        </target>
    </heartbeat>
</configSet>
```
#### ARRAY NTB XML
```xml
<run>
	<filter id="1" sessionBase="no">
		<or>
			<find name="tcp.port" relation="==" content="443"/>
			<find name="udp.port" relation="==" content="53"/>
		</or>
	</filter>
	<filter id="2" sessionBase="no">
		<or>
			<find name="tcp.port" relation="==" content="443"/>
			<find name="tcp.port" relation="==" content="20443"/>
			<find name="udp.port" relation="==" content="53"/>
		</or>
	</filter>
	<filter id="3" sessionBase="no">
		<or>
			<find name="ip.addr" relation="==" content="10.110.185.0/24"/>
		</or>
	</filter>
	<filter id="100" sessionBase="no">
		<or>
			<find name="heartbeat.target.miss.id" relation="==" content="1"/>
			<find name="heartbeat.target.miss.id" relation="==" content="2"/>
		</or>
	</filter>
	<filter id="101" sessionBase="no">
		<or>
			<find name="heartbeat.target.miss.id" relation="==" content="3"/>
		</or>
	</filter>
	<chain>
		<in>P6</in>
		<fid>F101</fid>
		<out>P7</out>
		<next type="notmatch">
			<fid>F100</fid>
			<out>P4</out>
			<next type="notmatch">
				<fid type="and">F1,F3</fid>
				<out>P0</out>
				<next type="notmatch">
					<out>P4</out>
				</next>
			</next>
		</next>
	</chain>
	<chain>
		<in>P7</in>
		<fid>F101</fid>
		<out>P6</out>
		<next type="notmatch">
			<fid>F100</fid>
			<out>P5</out>
			<next type="notmatch">
				<fid type="and">F1,F3</fid>
				<out>P1</out>
				<next type="notmatch">
					<out>P5</out>
				</next>
			</next>
		</next>
	</chain>
	<chain>
		<in>P4</in>
		<fid>F100</fid>
		<out>P6</out>
		<next type="notmatch">
			<fid type="and">F2,F3</fid>
			<out>P2</out>
			<next type="notmatch">
				<out>P6</out>
			</next>
		</next>
	</chain>
	<chain>
		<in>P5</in>
		<fid>F100</fid>
		<out>P7</out>
		<next type="notmatch">
			<fid type="and">F2,F3</fid>
			<out>P3</out>
			<next type="notmatch">
				<out>P7</out>
			</next>
		</next>
	</chain>
	<chain>
		<in>P0</in>
		<out>P6</out>
	</chain>
	<chain>
		<in>P1</in>
		<out>P7</out>
	</chain>
	<chain>
		<in>P2</in>
		<out>P4</out>
	</chain>
	<chain>
		<in>P3</in>
		<out>P5</out>
	</chain>
</run>
```

## Service Chain with SSLi(A10) and IPS
```
                         WAN
                          |
                          |P7 
                    ---------------         ----------
                   |               | P1----|          |
  ---------        |               |       |    A10   |
 |         |----P5 |               | P3----|          |
 |   IPS   |       |   Array NTB   |        ----------
 |         |----P4 |               |        ----------
  ---------        |               | P2----|          |
                   |               |       |    A10   |
                   |               | P0----|          |
                    ---------------         ----------
                           |P6
                           |
                          LAN
```     
#### Config XML
```xml
<configSet reboot="no">
    <heartbeat>
        <enable>True</enable>
        <frequency>500</frequency>
        <maxAllowTimeouts>3</maxAllowTimeouts>
        <target>
            <enable>True</enable>
            <sendPort>P0</sendPort>
            <receivePort>P2</receivePort>
            <packetData>000d48285134000d482851338137ffff0030000000004004eca2c6130102c61301010000000000000000000000000000000000000000000000000000</packetData>
            <description>Array1</description>
            <id>1</id>
        </target>
	    <target>
            <enable>True</enable>
            <sendPort>P1</sendPort>
            <receivePort>P3</receivePort>
            <packetData>000d48285134000d482851338137ffff0030000000004004eca2c6130102c61301010000000000000000000000000000000000000000000000000000</packetData>
            <description>Array2</description>
            <id>2</id>
        </target>
	    <target>
            <enable>True</enable>
            <sendPort>P4</sendPort>
            <receivePort>P5</receivePort>
            <packetData>000d48285134000d482851338137ffff0030000000004004eca2c6130102c61301010000000000000000000000000000000000000000000000000000</packetData>
            <description>IPS</description>
            <id>3</id>
        </target>
    </heartbeat>
</configSet>
```
#### ARRAY NTB XML
```xml
<run>
	<filter id="1" sessionBase="no">
		<or>
			<find name="tcp.port" relation="==" content="443"/>
		</or>
	</filter>
	<filter id="2" sessionBase="no">
		<or>
			<find name="tcp.port" relation="==" content="443"/>
			<find name="tcp.port" relation="==" content="8080"/>
		</or>
	</filter>
	<filter id="3" sessionBase="no">
		<or>
			<find name="ip.addr" relation="==" content="192.168.1.0/24"/>
		</or>
	</filter>
	<filter id="99" sessionBase="no">
		<or>
			<find name="ip.proto" relation="==" content="1"/>
			<find name="arp" relation="==" content=""/>
		</or>
	</filter>
	<filter id="100" sessionBase="no">
		<or>
			<find name="heartbeat.target.miss.id" relation="==" content="1"/>
			<find name="heartbeat.target.miss.id" relation="==" content="2"/>
		</or>
	</filter>
	<filter id="101" sessionBase="no">
		<or>
			<find name="heartbeat.target.miss.id" relation="==" content="3"/>
		</or>
	</filter>
	<chain>
		<in>P6</in>
		<fid>F101</fid>
		<out>P7</out>
		<next type="notmatch">
			<fid>F100</fid>
			<out>P4</out>
			<next type="notmatch">
				<fid type="and">F1,F3</fid>
				<out>P0</out>
				<next type="notmatch">
					<out>P4</out>
				</next>
			</next>
		</next>
	</chain>
	<chain>
		<in>P7</in>
		<fid>F101</fid>
		<out>P6</out>
		<next type="notmatch">
			<fid>F100</fid>
			<out>P5</out>
			<next type="notmatch">
				<fid type="and">F1,F3</fid>
				<out>P1</out>
				<next type="notmatch">
					<out>P5</out>
				</next>
			</next>
		</next>
	</chain>
	<chain>
		<in>P4</in>
		<fid>F100</fid>
		<out>P6</out>
		<next type="notmatch">
			<fid type="and">F2,F3</fid>
			<out>P2</out>
			<next type="notmatch">
				<out>P6</out>
			</next>
		</next>
	</chain>
	<chain>
		<in>P5</in>
		<fid>F100</fid>
		<out>P7</out>
		<next type="notmatch">
			<fid type="and">F2,F3</fid>
			<out>P3</out>
			<next type="notmatch">
				<out>P7</out>
			</next>
		</next>
	</chain>
	<chain>
		<in>P0</in>
		<out>P6</out>
	</chain>
	<chain>
		<in>P1</in>
		<out>P7</out>
	</chain>
	<chain>
		<in>P2</in>
		<out>P4</out>
	</chain>
	<chain>
		<in>P3</in>
		<out>P5</out>
	</chain>
	<!-- dup icmp and arp for A10 heartbeat check-->
	<chain>
		<in>P4</in>
		<fid>F99</fid>
		<out>P2</out>
	</chain>
	<chain>
		<in>P5</in>
		<fid>F99</fid>
		<out>P3</out>
	</chain>
	<chain>
		<in>P6</in>
		<fid>F99</fid>
		<out>P0</out>
	</chain>
	<chain>
		<in>P7</in>
		<fid>F99</fid>
		<out>P1</out>
	</chain>
</run>
```
