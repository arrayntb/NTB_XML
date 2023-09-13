---
description: Defines the chain. It has a start tag <chain> and an end tag </chain>.
---

# Element \<chain>

## Attribute

<table><thead><tr><th width="179">Attribute</th><th width="212.21262002743487">Description</th><th width="150">Type</th><th>Default (* must have)</th></tr></thead><tbody><tr><td>id</td><td>Specifies a unique id for an element</td><td>Interger</td><td></td></tr><tr><td>name</td><td>Specifies a name for an element</td><td>String</td><td></td></tr><tr><td>sessionUnique</td><td>one session one output</td><td>yes/no</td><td>no</td></tr><tr><td>disable</td><td>disable chain</td><td>yes/no</td><td>no</td></tr></tbody></table>

## Elements in chain

### \<in>

Defines input ports. It has a start tag \<in> and an end tag \</in>.

#### Example

```xml
<in>P0,P1</in>
```

### \<out>

Defines output ports. It has a start tag \<out> and an end tag \</out>.

#### Predefined

* 0: Drop
* S: find destination by dst-mac address (like Switch port)

#### Attribute

<table><thead><tr><th width="150">Attribute</th><th width="249.7142857142857">Description</th><th width="150">Type</th><th>Default (* must have)</th></tr></thead><tbody><tr><td>type</td><td>duplicate or loadBalance</td><td>String</td><td>duplicate</td></tr><tr><td>lbtype</td><td>Load Balance type, includes session, ethtype, iptype, smac, dmac, sip, dip, rr, 5thash</td><td>String</td><td>session</td></tr><tr><td>failover</td><td>Load Balance fail over</td><td>yes/no</td><td>yes</td></tr><tr><td>weight</td><td>Load Balance weight (not support session, rr type)</td><td>String</td><td>20,80</td></tr></tbody></table>

#### Example

```xml
<!--duplication to P1 and P2 -->
<out>P0,P1</out>
<!-- load Balance to P0,P1 by 5-tuple -->
<out type="loadBalance" lbtype="5thash">P0,P1</out>
```

### \<fid>

Defines packets pass through filter id. It has a start tag \<fid> and an end tag \</fid>.

#### Attribute

<table><thead><tr><th width="150">Attribute</th><th width="150">Description</th><th>Type</th><th>Default (* must have)</th></tr></thead><tbody><tr><td>type</td><td>and/or</td><td>String</td><td>or</td></tr></tbody></table>

#### Example

```xml
  <fid>F1</fid>
<!--if F1 or F2 -->
  <fid type="or">F1,F2</fid>

<!--if F1 and not F2 -->
  <fid type="and">F1,!F2</fid>
```

### \<next>

Defines going next if packet match/not match filter. It has a next tag \<next> and an end tag \</next>.

#### Attribute

<table><thead><tr><th width="150">Attribute</th><th>Description</th><th>Type</th><th>Default (* must have)</th></tr></thead><tbody><tr><td>type</td><td>match/notmatch</td><td>String</td><td>match</td></tr></tbody></table>

#### Example

```xml
<!-- packet from P0, if matched F1 (if matched F2, send to P1) else (if match F3, send to P2) -->
<chain>
  <in>P0</in> 
  <fid>F1</fid>
  <next>
    <fid>F2</fid>
    <out>P1</out>
  </next>
  <next type="notmatch">
    <fid>F3</fid>
    <out>P2</out>
  </next>
</chain>
```

## Example

P0->F1->P1

```xml
<chain id="1">
  <in>P0</in>
  <fid>F1</fid>
  <out>P1</out>
</chain>
```

P0->F1->F2->P1\
&#x20;                  !>P2

```xml
<chain id="1">
  <in>P0</in>
  <fid>F1</fid>
  <next>
    <fid>F2</fid>
    <out>P1</out>
    <next type="notmatch">
      <out>P2</out>
    </next>
  </next>
</chain>
```
