---
description: IP/domain/url/ssl server_name Block/Detect Sample
---

# Block/Detect black list

### Config XML

send syslog to log server 192.168.1.12:514 if block/detect happened

<pre class="language-xml"><code class="lang-xml"><strong>&#x3C;configSet reboot="no">
</strong>    &#x3C;log>
        &#x3C;syslog>
            &#x3C;enable>True&#x3C;/enable>
            &#x3C;port>M0&#x3C;/port>
            &#x3C;target>
                &#x3C;enable>True&#x3C;/enable>
                &#x3C;dip>192.168.1.12&#x3C;/dip>
                &#x3C;dport>514&#x3C;/dport>
                &#x3C;interfaces>P6,P7&#x3C;/interfaces>
                &#x3C;filter>&#x3C;/filter>
                &#x3C;type>matched&#x3C;/type>
                &#x3C;subtype>
                    &#x3C;sip>True&#x3C;/sip>
                    &#x3C;dip>True&#x3C;/dip>
                    &#x3C;sport>True&#x3C;/sport>
                    &#x3C;dport>True&#x3C;/dport>
                    &#x3C;protocol>True&#x3C;/protocol>
                    &#x3C;find_id>True&#x3C;/find_id>
                    &#x3C;find_content>True&#x3C;/find_content>
                &#x3C;/subtype>
            &#x3C;/target>
        &#x3C;/syslog>
    &#x3C;/log>
&#x3C;/configSet>
</code></pre>

### ARRAY NTB XML (black list sample)

```xml
<run>
    <filter id="10000" sessionBase="yes" matchedlog="yes">
        <or>
            <find id="10000" name="ip.addr" relation="==" content="8.8.8.8"/>
        </or>
    </filter>
    <filter id="10001" sessionBase="no" matchedlog="yes">
        <or>
            <find id="10002" name="dns.qry.name" relation="==" content="www.cittv.com.tw"/>
        </or>
    </filter>
    <filter id="10002" sessionBase="no" matchedlog="yes">
        <or>
            <find id="10004" name="http.request.url" relation="==" content="www.whitehollowtransport.com/current-elliott-c-89.html" />
        </or>
    </filter>
    <filter id="10003" sessionBase="no" matchedlog="yes">
        <or>
            <find id="10005" name="ssl.server_name" relation="==" content="facebook.com" />
        </or>
    </filter>
    <filter id="10004" sessionBase="no" matchedlog="yes">
        <or>
            <find id="10006" name="ssl.server_name_public_suffix" relation="==" content=" *.googlevideo.com" />
        </or>
    </filter>
</run>
```

### ARRAY NTB XML(block sample)

```xml
<run>
    <chain>
        <in>P6</in>
        <fid>F10000,F10001,F10002,F10003,F10004</fid>
        <out>0</out>
        <next type=”notmatch”>
            <out>P7</out>
        </next>
    </chain>
    <chain>
        <in>P7</in>
        <fid>F10000,F10001,F10002,F10003,F10004</fid>
        <out>0</out>
        <next type=”notmatch”>
            <out>P6</out>
        </next>
    </chain>
</run>
```

### ARRAY NTB XML(detect sample)

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
    <chain>
        <in>P6,P7</in>
        <fid>F10000,F10001,F10002,F10003,F10004</fid>
        <out>0</out>
    </chain>
</run>
```
