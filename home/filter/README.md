---
description: Defines the filter. It has a start tag <filter> and an end tag </filter>.
---

# Element \<filter>

In filter, must start at \<or>\</or> or \<and>\</and>, then put \<find/> into there like

And filter id=1 -> F1, refer to Example

```xml
<filter id="1" >
    <or>
	<find name="tcp.port" relation="==" content="443" />
	<find name="udp.port" relation="==" content="53" />
    </or>
</filter>
```

### \<or>

Defines all finds conjunct with or. It has a start tag \<or> and an end tag \</or>.

### \<and>

Defines all finds conjunct with and. It has a start tag \<and> and an end tag \</and>.

### \<find>

Please refer to [Element - \<find](find.md)>

## Attribute

<table><thead><tr><th width="188">Attribute</th><th width="387">Description</th><th width="150">Type</th><th width="154">Default (* must have)</th><th>Support</th></tr></thead><tbody><tr><td>id</td><td>Specifies a unique id for an element</td><td>Interger</td><td>*</td><td></td></tr><tr><td>name</td><td>Specifies a name for an element</td><td>String</td><td></td><td></td></tr><tr><td>sessionBase</td><td>If one packet in session match filter, the whole session will treat as match</td><td>yes/no</td><td>yes</td><td></td></tr><tr><td>matchedlog</td><td>if match filter and syslog set, send log</td><td>yes/no</td><td>no</td><td></td></tr><tr><td>mpslog</td><td>if matched per second over value, send log</td><td>Interger</td><td>0</td><td>v5.3</td></tr><tr><td>blockifempty</td><td>block if no find in filter</td><td>yes/no</td><td>no</td><td></td></tr><tr><td>maxPackets</td><td>only match first N packets in a session</td><td>Interger</td><td>0(means no limit)</td><td></td></tr><tr><td>start</td><td>start filter position, support l2, l3, l4, l7, http_body</td><td>String</td><td>l7</td><td>v5.3, regex only</td></tr><tr><td>position</td><td>absolute position for filter</td><td>Interger</td><td>-1</td><td>v5.3, regex only</td></tr><tr><td>within</td><td>maxinum size for filter</td><td>Interger</td><td>0</td><td>v5.3, regex only</td></tr><tr><td>masking</td><td>just masking, no filter function</td><td>yes/no</td><td>no</td><td>regex only</td></tr><tr><td>tuple5_live_hashtable_size</td><td>set hash table size for tuple5 live use only</td><td>Interger</td><td>no</td><td>v3.3</td></tr></tbody></table>

## Example

filter dns port and server

```xml
<run>
<filter id="1" sessionBase="no">
    <or>
	<find name="udp.port" relation="==" content="53" />
    </or>
</filter>
<filter id="2" sessionBase="no">
    <or>
	<find name="ip.addr" relation="==" content="8.8.8.8" />
	<find name="ip.addr" relation="==" content="8.8.4.4" />
	<find name="ip.addr" relation="==" content="168.95.1.1" />
	<find name="ip.addr" relation="==" content="168.95.100.1" />
    </or>
</filter>
<chain>
    <in>P0</in>
    <fid type="and">F1,F2</fid>
    <out>P1</out>
    <next type="notmatch">
        <out>P2</out>
    </next>
</chain>
</run>
```

## Example for Regular Expression

```xml
<run>
    <filter id="1" sessionBase="no" start="l7" within="100">
        <or>
            <!-- string -->
            <find name="regex" relation="==" content="This is p@cket filtering test"/>
            <!-- hex -->
            <find name="regex" relation="==" content="\xE9\xA2\xB1\xE9\xA2\xA8"/>
            <!-- http host name-->
            <find name="regex" relation="==" content="{s}\/.*Host: nlpqflkbvkdde\.eu"/>
            <!-- dns qry name-->
            <find name="regex" relation="==" content="\x08facebook\x03com"/>
        </or>
    </filter>
    <chain>
        <in>P0</in>
        <fid>F1</fid>
        <out>P1</out>
    </chain>
</run>
```
