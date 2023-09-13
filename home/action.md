---
description: Defines the Action. It has a start tag <action> and an end tag </action>.
---

# Element \<action>

Two main funcitons, "input packet process" and "link pairs".

## Attribute

<table><thead><tr><th width="150">Attribute</th><th width="233.7142857142857">Description</th><th width="150">Type</th><th>Default (* must have)</th></tr></thead><tbody><tr><td>id</td><td>Specifies a unique id for an element</td><td>Interger</td><td>*</td></tr><tr><td>name</td><td>Specifies a name for an element</td><td>String</td><td></td></tr><tr><td>type</td><td>action type</td><td>String</td><td>input-packet-process or linkpairs</td></tr></tbody></table>

## Example

```xml
<run>
  <action id="1" type="input-packet-process">
    <port>P0</port>
    <stripping>vlan</stripping>
  </action>
</run>
```

## Elements in Action

### input-packet-process type

### \<port>

Defines input port(must have). It has a start tag \<port> and an end tag \</port>.

```xml
<action id="1" type="input-packet-process">
  <port>P0</port>
</action>
```

### \<Q>

Defines vlan tagging. It has a start tag \<Q> and an end tag \</Q>.

```xml
<action id="1" type="input-packet-process">
  <port>P0</port>
  <Q>10</Q>
</action>
```

### \<QinQ>

Defines vlan layer 2 tagging. It has a start tag \<QinQ> and an end tag \</QinQ>.

```xml
<action id="1" type="input-packet-process">
  <port>P0</port>
  <QinQ>20</QinQ>
</action>
```

### \<stripping>

Defines stripping. It has a start tag \<stripping> and an end tag \</stripping>.

#### support type

* payload
* vlan
* mpls
* gre
* vxlan
* gre-erspan
* gtp
* grism
* mpls-in-udp
* mpls-in-gre

```xml
<action id="1" type="input-packet-process">
  <port>P0</port>
  <stripping>vlan</stripping>
</action>
```

### \<tagging>

Defines tagging. It has a start tag \<tagging> and an end tag \</tagging>.

#### support type

* timestamp
* gtp
* gtp2
* grism

```xml
<action id="1" type="input-packet-process">
  <port>P0</port>
  <tagging>grism</taging>
</action>
```

### \<maxlen>

Defines packet max length. It has a start tag \<maxlen> and an end tag \</maxlen>.

```xml
<action id="1" type="input-packet-process">
  <port>P0</port>
  <maxlen>64</maxlen>
</action>
```

### linkpairs type

If portA down, force portB down, and vice versa.

```xml
<action id="1" type="linkpairs">
  <portA>P1</portA>
  <portB>P2</portB>
</action>
```
