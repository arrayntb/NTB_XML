---
description: Offload network analysis device
---

# Offload

### Only send TCP/UDP 443 first 20 packets to analysis device

<pre class="language-xml"><code class="lang-xml"><strong>&#x3C;run>
</strong>  &#x3C;filter id="1" maxPackets="20" sessionBase="no">
    &#x3C;or>
      &#x3C;find name="tcp.port" relation="==" content="443"/>
      &#x3C;find name="udp.port" relation="==" content="443"/>
    &#x3C;/or>
  &#x3C;/filter>
  &#x3C;chain>
    &#x3C;in>P1&#x3C;/in>
    &#x3C;fid>F1&#x3C;/fid>
    &#x3C;out>P2&#x3C;/out>
  &#x3C;/chain>
&#x3C;/run></code></pre>

### Remove Youtube/Netflix/tiktok/Teams/Zoom/WebEx/Spotify streaming&#x20;

#### Reference

* https://github.com/ntop/nDPI/blob/dev/src/lib/ndpi\_content\_match.c.inc
* https://github.com/boychongzen18/Bug-Host-All-Operator

```xml
<run>
  <filter id="1">
    <or>
      <!-- youtube -->
      <find name="ssl.server_name" relation="==" content="upload.youtube.com"/>
      <find name="ssl.server_name" relation="==" content="youtubei.googleapis.com"/>
      <find name="ssl.server_name" relation="==" content="youtu.be"/>
      <find name="ssl.server_name" relation="==" content="yt3.ggpht.com"/>
      <!-- netflix -->
      <find name="ssl.server_name" relation="==" content="netflix.com"/>
      <find name="ssl.server_name" relation="==" content="nflxext.com"/>
      <find name="ssl.server_name" relation="==" content="nflximg.com"/>
      <find name="ssl.server_name" relation="==" content="nflximg.net"/>
      <find name="ssl.server_name" relation="==" content="nflxvideo.net"/>
      <find name="ssl.server_name" relation="==" content="nflxso.net"/>
      <find name="ssl.server_name" relation="==" content="fast.com"/> 
      <!-- Teams -->
      <find name="ssl.server_name" relation="==" content="teams.microsoft.com"/>
      <find name="ssl.server_name" relation="==" content="teams.microsoft.us"/>
      <find name="ssl.server_name" relation="==" content="teams.skype.com"/>
      <find name="ssl.server_name" relation="==" content="-teams.cloudapp.net"/>
      <find name="ssl.server_name" relation="==" content="teams.trafficmanager.net"/>
      <find name="ssl.server_name" relation="==" content="teams-msgapi.trafficmanager.net"/>
      <find name="ssl.server_name" relation="==" content="teams.office.net"/>
      <find name="ssl.server_name" relation="==" content="teams.office.com"/>
      <!-- Spotify -->
      <find name="ssl.server_name" relation="==" content="spotifycdn.net"/>
      <find name="ssl.server_name" relation="==" content="spotifycdn.com"/>
      <find name="ssl.server_name" relation="==" content="audio4-ak-spotify-com.akamaized.net"/>
      <find name="ssl.server_name" relation="==" content="audio-ak-spotify-com.akamaized.net"/>
      <find name="ssl.server_name" relation="==" content="heads-ak-spotify-com.akamaized.net"/>
      <find name="ssl.server_name" relation="==" content="spotify-com.akamaized.net"/>
      <find name="ssl.server_name" relation="==" content="spotify.com.edgesuite.net"/>
      <find name="ssl.server_name" relation="==" content="spotify.map.fastly.net"/>
      <find name="ssl.server_name" relation="==" content="spotify.edgekey.net"/>
      <find name="ssl.server_name" relation="==" content="spotify.demdex.net"/>
      <!-- tiktok -->
      <find name="ssl.server_name" relation="==" content="tiktok.com"/>
    </or>
  </filter>
  <filter id="2">
    <or>
      <!-- youtube -->
      <find name="ssl.server_name_public_suffix" relation="==" content="*.googlevideo.com"/>
      <find name="ssl.server_name_public_suffix" relation="==" content="*.ytimg.com"/>
      <find name="ssl.server_name_public_suffix" relation="==" content="*.youtube-ui.l.google.com"/>
      <find name="ssl.server_name_public_suffix" relation="==" content="*.youtubeeducation.com"/>
      <!-- Zoom -->
      <find name="ssl.server_name_public_suffix" relation="==" content="*.zoom.us"/>
      <!-- WebEx -->
      <find name="ssl.server_name_public_suffix" relation="==" content="*.webex.com"/>
      <!-- Spotify -->
      <find name="ssl.server_name_public_suffix" relation="==" content="*.spotify.com"/>
      <find name="ssl.server_name_public_suffix" relation="==" content="*.scdn.co"/>
      <find name="ssl.server_name_public_suffix" relation="==" content="*.pscdn.co"/>
      <find name="ssl.server_name_public_suffix" relation="==" content="*.spotilocal.com"/> 
      <!-- tiktok -->
      <find name="ssl.server_name_public_suffix" relation="==" content="*.tiktok.com"/>
      <find name="ssl.server_name_public_suffix" relation="==" content="*.tiktokcdn.com"/>
    </or>
  </filter>
  <chain>
    <in>P0</in>
    <fid>F1,F2</fid>
    <out>0</out>
    <next type="notmatch">
      <out>P1</out>
    </next>
  </chain>
</run>
```
