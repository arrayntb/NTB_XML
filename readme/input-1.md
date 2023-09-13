---
description: Defines the Input. It has a start tag <input> and an end tag </input>.
---

# Element \<input>

Two main funcitons, "replay pcap files" and "traffic generator".

## Attribute

<table><thead><tr><th width="150">Attribute</th><th width="249.79413377234155">Description</th><th width="150">Type</th><th>Default (* must have)</th></tr></thead><tbody><tr><td>id</td><td>Specifies a unique id for an element</td><td>Interger</td><td></td></tr><tr><td>type</td><td>Specifies a type for an element</td><td>String</td><td>replayPcap or traffic-gen</td></tr></tbody></table>

## Elements in Input - replayPcap

Before using this function, make sure you upload pcap files to the correct path first.

### Example

```xml
<run>
    <input type="replayPcap">
        <port>P0</port>
        <filepath>H1/in/sample.pcap</filepath>
        <time>1</time>
        <msinterval>1</msinterval>   
    </input>
</run>
```

### port

Defines output port\*\*(must have)\*\*.

It has a start tag \<port> and an end tag \</port>.

```xml
  <port>P0</port>
```

### time

Defines play time\*\*(must have)\*\*.

It has a start tag \<time> and an end tag \</time>.

```xml
<time>1</time>
```

### filepath

Defines pcap filepath\*\*(either filepath or scandir must have)\*\*.

It has a start tag \<filepath> and an end tag \</filepath>.

```xml
<filepath>H1/in/sample.pcap</filepath>
```

### speed

Defines speed, default is full line rate.

It has a start tag \<speed> and an end tag \</speed>.

```xml
<speed>10000</speed>
```

### msinterval

Defines the play ms interval between each packet.

It has a start tag \<msinterval> and an end tag \</msinterval>.

```xml
<msinterval>1</msinterval>
```

### scandir

Defines pcap scandir\*\*(either filepath or scandir must have)\*\*.

It has a start tag \<scandir> and an end tag \</scandir>., limit 1024 files

#### Attribute

<table><thead><tr><th width="150">Attribute</th><th width="237.7142857142857">Description</th><th width="150">Type</th><th>Default (* must have)</th></tr></thead><tbody><tr><td>interval</td><td>Scan interval</td><td>Seconds</td><td>60</td></tr><tr><td>minbytes</td><td>will replay if pcap file bigger than minbytes</td><td>Interger</td><td>0</td></tr><tr><td>timeout</td><td>force replay if pcap file less than minbytes after timeout</td><td>Interger</td><td>0</td></tr></tbody></table>

```xml
<scandir interval="10" minbytes="1048576" timeout="60">H1/in</scandir>
```

### playedFilesHandle

Defines pcap file handle after replay.

It has a start tag \<playedFilesHandle> and an end tag \</playedFilesHandle>. must be **delete** or **move**

```xml
<playedFilesHandle>move</playedFilesHandle>
```

### playedFilesMoveTo

Defines pcap file move to dir after after replay.

It has a start tag \<playedFilesMoveTo> and an end tag \</playedFilesMoveTo>.

```xml
<playedFilesMoveTo>H1/in/played</playedFilesMoveTo>
```

### Example - scandir

<pre class="language-xml"><code class="lang-xml">&#x3C;run>
<strong>    &#x3C;input type="replayPcap">
</strong>        &#x3C;port>P0&#x3C;/port>
        &#x3C;time>1&#x3C;/time>
        &#x3C;scandir interval="10" minbytes="1048576" timeout="60">H1/in&#x3C;/scandir>
        &#x3C;playedFilesHandle>move&#x3C;/playedFilesHandle>
        &#x3C;playedFilesMoveTo>H1/played&#x3C;/playedFilesMoveTo>  
    &#x3C;/input>
&#x3C;/run>
</code></pre>

## Elements in Input - traffic-gen

generate traffic

### Example

```xml
<run>
  <input type="traffic-gen">
        <port>P0</port>
        <protocol>TCP</protocol>
        <packet_size>1024</packet_size>
        <speed>10000</speed>
        <payload_text>abcdefg</payload_text>
        <src_mac>00:0d:48:28:28:56</src_mac>
        <dest_mac>00:0d:48:28:28:57</dest_mac>  
        <src_ip>10.1.0.99</src_ip>
        <src_ip_min>10.1.0.0</src_ip_min>
        <src_ip_max>10.1.0.99</src_ip_max>
        <src_ip_inc>5</src_ip_inc>
        <src_ip_random>0</src_ip_random>
        <dest_ip>11.1.1.99</dest_ip>
        <dest_ip_min>11.1.1.0</dest_ip_min>
        <dest_ip_max>11.1.2.99</dest_ip_max>
        <dest_ip_inc>2</dest_ip_inc>
        <dest_ip_random>0</dest_ip_random>
        <src_port>1234</src_port>
        <src_port_min>2</src_port_min>
        <src_port_max>9999</src_port_max>
        <src_port_inc>1</src_port_inc>
        <src_port_random>0</src_port_random>
        <dest_port>2222</dest_port>
        <dest_port_min>0</dest_port_min>
        <dest_port_max>65535</dest_port_max>
        <dest_port_inc>1</dest_port_inc>
        <dest_port_random>0</dest_port_random>
    </input>
</run>
```

### port

Defines output port\*\*(must have)\*\*.

It has a start tag \<port> and an end tag \</port>.

```xml
  <port>P0</port>
```

### protocol

Defines protocol TCP/UDP/ICMP, default UDP

It has a start tag \<protocol> and an end tag \</protocol>.

```xml
<protocol>UDP</protocol>
```

### packet\_size

Defines packet size, default 512

It has a start tag \<packet\_size> and an end tag \</packet\_size>.

```xml
<packet_size>1024</packet_size>
```

### packet\_data

Defines packet data

It has a start tag \<packet\_data> and an end tag \</packet\_data>.

```xml
<packet_data>000cbd0bfd36000cbd0bfd3708004500002e00000000800124090a0001630a0001640800662f0001000168656c6c6f20776f726c6400000000000000</packet_data>
```

### speed

Defines speed, default is full line rate.

It has a start tag \<speed> and an end tag \</speed>.

```xml
<speed>10000</speed>
```

### msinterval

Defines the ms interval between each packet. higher priority than speed.

It has a start tag \<msinterval> and an end tag \</msinterval>.

```xml
<msinterval>1</msinterval>
```

### payload\_text

Defines payload text

It has a start tag \<payload\_text> and an end tag \</payload\_text>.

```xml
<payload_text>abcdefg</payload_text>
```

### src\_mac

Defines source mac address

It has a start tag \<src\_mac> and an end tag \</src\_mac>.

```xml
<src_mac>00:0d:48:28:28:56</src_mac>
```

### dest\_mac

Defines destination mac address

It has a start tag \<dest\_mac> and an end tag \</dest\_mac>.

```xml
<dest_mac>00:0d:48:28:28:57</dest_mac>
```

### src\_ip

Defines source ip, default 10.0.1.99

It has a start tag \<src\_ip> and an end tag \</src\_ip>.

```xml
<src_ip>10.1.0.99</src_ip>
```

### src\_ip\_min

Defines minimum source ip

It has a start tag \<src\_ip\_min> and an end tag \</src\_ip\_min>.

```xml
<src_ip_min>10.1.0.0</src_ip_min>
```

### src\_ip\_max

Defines maximum source ip

It has a start tag \<src\_ip\_max> and an end tag \</src\_ip\_max>.

```xml
<src_ip_max>10.1.0.99</src_ip_max>
```

### src\_ip\_inc

Defines the number to increase source ip, default 0

It has a start tag \<src\_ip\_inc> and an end tag \</src\_ip\_inc>.

```xml
<src_ip_inc>5</src_ip_inc>
```

### src\_ip\_random

Defines source ip random (0 or 1), default 0

It has a start tag \<src\_ip\_random> and an end tag \</src\_ip\_random>.

```xml
<src_ip_random>1</src_ip_random>
```

### src\_port

Defines source port, default 5000

It has a start tag \<src\_port> and an end tag \</src\_port>.

```xml
<src_port>1234</src_port>
```

### src\_port\_min

Defines minimum source port

It has a start tag \<src\_port\_min> and an end tag \</src\_port\_min>.

```xml
<src_port_min>2</src_port_min>
```

### src\_port\_max

Defines maximum source port

It has a start tag \<src\_port\_max> and an end tag \</src\_port\_max>.

```xml
<src_port_max>9999</src_port_max>
```

### src\_port\_inc

Defines the number to increase source port, default 0

It has a start tag \<src\_port\_inc> and an end tag \</src\_port\_inc>.

```xml
 <src_port_inc>10</src_port_inc>
```

### src\_port\_random

Defines random source port (0 or 1), default 0

It has a start tag \<src\_port\_random> and an end tag \</src\_port\_random>.

```xml
 <src_port_random>1</src_port_random>
```

### dest\_ip

Defines destination ip,default 10.0.0.99

### dest\_ip\_min

Defines minimum destination ip

### dest\_ip\_max

Defines maximum destination ip

### dest\_ip\_inc

Defines the number to increase destination ip, default 0

### dest\_ip\_random

Defines destination ip random (0 or 1), default 0

### dest\_port

Defines destination port, default 5001

### dest\_port\_min

Defines minimum destination port

### dest\_port\_max

Defines maximum destination port

### dest\_port\_inc

Defines the number to increase destination port, default 0

### dest\_port\_random

Defines random destination port (0 or 1), default 0

//destination ip and port, please refer to src\_ip and src\_port for xml syntax
