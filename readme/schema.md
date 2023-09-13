---
description: ARRAY XML Schema
---

# Schema

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">

  <xs:element name="run">
    <xs:complexType>
      <xs:choice minOccurs="0" maxOccurs="unbounded">
        <xs:element name="script">
            <xs:complexType>
              <xs:simpleContent>
                <xs:extension base="xs:string">
                  <xs:attribute name="src" type="xs:string"></xs:attribute>
                </xs:extension>
              </xs:simpleContent>
            </xs:complexType>
        </xs:element>
        <xs:element ref="filter"></xs:element>
        <xs:element ref="input"></xs:element>
        <xs:element ref="action"></xs:element>
        <xs:element ref="output"></xs:element>
        <xs:element ref="chain"></xs:element>
        <xs:element ref="bandwidthReserve"></xs:element>
      </xs:choice>
      <xs:attribute name="id" type="xs:integer"></xs:attribute>
    </xs:complexType>
    <xs:key name="filter-id">
      <xs:selector xpath="filter"></xs:selector>
      <xs:field xpath="@id"></xs:field>
    </xs:key>
    <xs:key name="output-id">
      <xs:selector xpath="output"></xs:selector>
      <xs:field xpath="@id"></xs:field>
    </xs:key><!--
    <xs:key name="keyChain">
      <xs:selector xpath="chain"></xs:selector>
      <xs:field xpath="@id"></xs:field>
    </xs:key>-->
    <xs:key name="action-id">
      <xs:selector xpath="action"></xs:selector>
      <xs:field xpath="@id"></xs:field>
    </xs:key>
    <xs:key name="bandwidth-reserve-id">
      <xs:selector xpath="bandwidthReserve"></xs:selector>
      <xs:field xpath="@id"></xs:field>
    </xs:key>
  </xs:element>

  <xs:simpleType name="ynType">
    <xs:restriction base="xs:token">
      <xs:enumeration value="yes"></xs:enumeration>
      <xs:enumeration value="no"></xs:enumeration>
    </xs:restriction>
  </xs:simpleType>

  <xs:simpleType name="t1f0Type">
    <xs:restriction base="xs:unsignedByte">
      <xs:enumeration value="0"></xs:enumeration>
      <xs:enumeration value="1"></xs:enumeration>
    </xs:restriction>
  </xs:simpleType>

  <xs:simpleType name="vlanidType">
    <xs:restriction base="xs:unsignedShort">
      <xs:maxExclusive value="4095"></xs:maxExclusive>
      <xs:minExclusive value="0"></xs:minExclusive>
    </xs:restriction>
  </xs:simpleType>

  <xs:simpleType name="vlanPriorityType">
    <xs:restriction base="xs:unsignedByte">
      <xs:minInclusive value="0"/>
      <xs:maxInclusive value="7"/>
    </xs:restriction>
  </xs:simpleType>

  <xs:simpleType name="vlanOpType">
    <xs:restriction base="xs:token">
      <xs:enumeration value="add"></xs:enumeration>
      <xs:enumeration value="replace"></xs:enumeration>
      <xs:enumeration value="remove"></xs:enumeration>
    </xs:restriction>
  </xs:simpleType>

  <xs:simpleType name="netPortType">
    <xs:restriction base="xs:unsignedShort">
      <xs:minInclusive value="0"/>
      <xs:maxInclusive value="65535"/>
    </xs:restriction>
  </xs:simpleType>

  <xs:simpleType name="ipType">
    <xs:restriction base="xs:string">
      <xs:pattern value="([0-9]|[1-9][0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5])[.]([0-9]|[1-9][0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5])[.]([0-9]|[1-9][0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5])[.]([0-9]|[1-9][0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5])([/]([0-9]|[1-2][0-9]|3[0-2]))?"></xs:pattern>
    </xs:restriction>
  </xs:simpleType>

  <xs:simpleType name="ipv6Type">
    <xs:restriction base="xs:string">
      <xs:pattern value="(([0-9a-fA-F]{1,4}:){7,7}[0-9a-fA-F]{1,4}|([0-9a-fA-F]{1,4}:){1,7}:|([0-9a-fA-F]{1,4}:){1,6}:[0-9a-fA-F]{1,4}|([0-9a-fA-F]{1,4}:){1,5}(:[0-9a-fA-F]{1,4}){1,2}|([0-9a-fA-F]{1,4}:){1,4}(:[0-9a-fA-F]{1,4}){1,3}|([0-9a-fA-F]{1,4}:){1,3}(:[0-9a-fA-F]{1,4}){1,4}|([0-9a-fA-F]{1,4}:){1,2}(:[0-9a-fA-F]{1,4}){1,5}|[0-9a-fA-F]{1,4}:((:[0-9a-fA-F]{1,4}){1,6})|:((:[0-9a-fA-F]{1,4}){1,7}|:)|fe80:(:[0-9a-fA-F]{0,4}){0,4}%[0-9a-zA-Z]{1,}|::(ffff(:0{1,4}){0,1}:){0,1}((25[0-5]|(2[0-4]|1{0,1}[0-9]){0,1}[0-9])\.){3,3}(25[0-5]|(2[0-4]|1{0,1}[0-9]){0,1}[0-9])|([0-9a-fA-F]{1,4}:){1,4}:((25[0-5]|(2[0-4]|1{0,1}[0-9]){0,1}[0-9])\.){3,3}(25[0-5]|(2[0-4]|1{0,1}[0-9]){0,1}[0-9]))"></xs:pattern>
    </xs:restriction>
  </xs:simpleType>

  <xs:simpleType name="macType">
    <xs:restriction base="xs:string">
      <xs:pattern value="([0-9A-Fa-f]{2}[:-]){5}([0-9A-Fa-f]{2})"></xs:pattern>
    </xs:restriction>
  </xs:simpleType>

  <xs:simpleType name="fieldType">
    <xs:restriction base="xs:string">
      <xs:enumeration value="eth.addr"></xs:enumeration>
      <xs:enumeration value="eth.src"></xs:enumeration>
      <xs:enumeration value="eth.dst"></xs:enumeration>
      <xs:enumeration value="eth.type"></xs:enumeration>
      <xs:enumeration value="vlan.id"></xs:enumeration>
      <xs:enumeration value="vlan.l2.id"></xs:enumeration>
      <xs:enumeration value="vlan.priority"></xs:enumeration>
      <xs:enumeration value="ip"></xs:enumeration>
      <xs:enumeration value="ip.addr"></xs:enumeration>
      <xs:enumeration value="ip.src"></xs:enumeration>
      <xs:enumeration value="ip.dst"></xs:enumeration>
      <xs:enumeration value="ip.proto"></xs:enumeration>
      <xs:enumeration value="ip.fragment"></xs:enumeration>
      <xs:enumeration value="ip.flags.df"><!-- 0 or 1 --></xs:enumeration>
      <xs:enumeration value="ip.flags.mf"><!-- 0 or 1 --></xs:enumeration>
      <xs:enumeration value="ip.dsfield"></xs:enumeration>
      <xs:enumeration value="ipv6"></xs:enumeration>
      <xs:enumeration value="ipv6.addr"></xs:enumeration>
      <xs:enumeration value="ipv6.src"></xs:enumeration>
      <xs:enumeration value="ipv6.dst"></xs:enumeration>
      <xs:enumeration value="ipv6.nxt"></xs:enumeration>
      <xs:enumeration value="tcp"></xs:enumeration>
      <xs:enumeration value="tcp.port"></xs:enumeration>
      <xs:enumeration value="tcp.srcport"></xs:enumeration>
      <xs:enumeration value="tcp.dstport"></xs:enumeration>
      <xs:enumeration value="tcp.flags.syn"></xs:enumeration>
      <xs:enumeration value="tcp.flags.ack"></xs:enumeration>
      <xs:enumeration value="tcp.flags.fin"></xs:enumeration>
      <xs:enumeration value="tcp.flags.reset"></xs:enumeration>
      <xs:enumeration value="udp"></xs:enumeration>
      <xs:enumeration value="udp.port"></xs:enumeration>
      <xs:enumeration value="udp.srcport"></xs:enumeration>
      <xs:enumeration value="udp.dstport"></xs:enumeration>
      <xs:enumeration value="sctp"></xs:enumeration>
      <xs:enumeration value="sctp.port"></xs:enumeration>
      <xs:enumeration value="sctp.srcport"></xs:enumeration>
      <xs:enumeration value="sctp.dstport"></xs:enumeration>
      <xs:enumeration value="5-tuple"></xs:enumeration>
      <xs:enumeration value="gtp.cp"></xs:enumeration>
      <xs:enumeration value="gtp.data"></xs:enumeration>
      <xs:enumeration value="gtp.data.by.s1ap.SubscriberProfileIDforRFP"></xs:enumeration>
      <xs:enumeration value="gtp.data.by.s1ap.CellIdentity"></xs:enumeration>
      <xs:enumeration value="gtp.imsi"></xs:enumeration>
      <xs:enumeration value="gtp.teid"></xs:enumeration>
      <xs:enumeration value="ip.addr.related.gtp.imsi"></xs:enumeration>
      <xs:enumeration value="gre"></xs:enumeration>
      <xs:enumeration value="erspan.spanid"></xs:enumeration>
      <xs:enumeration value="voip"></xs:enumeration>
      <xs:enumeration value="voip.account"></xs:enumeration>
      <xs:enumeration value="voip.from"></xs:enumeration>
      <xs:enumeration value="voip.to"></xs:enumeration>
      <xs:enumeration value="dns.a"></xs:enumeration>
      <xs:enumeration value="dns.flags.response"></xs:enumeration>
      <xs:enumeration value="dns.qry.name"></xs:enumeration>
      <xs:enumeration value="dns.qry.name.resp.ip.addr"></xs:enumeration>
      <xs:enumeration value="dns.qry.name_public_suffix"></xs:enumeration>
      <xs:enumeration value="dns.qry.type"></xs:enumeration>
      <xs:enumeration value="dns.count.add_rr"></xs:enumeration>
      <xs:enumeration value="http"></xs:enumeration>
      <xs:enumeration value="http.host"></xs:enumeration>
      <xs:enumeration value="http.request"></xs:enumeration>
      <xs:enumeration value="http.request.method"></xs:enumeration>
      <xs:enumeration value="http.request.uri"></xs:enumeration>
      <xs:enumeration value="http.request.url"></xs:enumeration>
      <xs:enumeration value="ssl"></xs:enumeration>
      <xs:enumeration value="ssl.handshake.type"></xs:enumeration>
      <xs:enumeration value="ssl.ja3_digest"></xs:enumeration>
      <xs:enumeration value="ssl.ja3s_digest"></xs:enumeration>
      <xs:enumeration value="ssl.server_name"></xs:enumeration>
      <xs:enumeration value="ssl.server_name_public_suffix"></xs:enumeration>
      <xs:enumeration value="ftp"></xs:enumeration>
      <xs:enumeration value="regex"></xs:enumeration>
      <xs:enumeration value="grism.port.linkdown"></xs:enumeration>
      <xs:enumeration value="grism.srcport"></xs:enumeration>
      <xs:enumeration value="session.packet.nth"></xs:enumeration>
      <xs:enumeration value="heartbeat.target.miss.id"></xs:enumeration>
      <xs:enumeration value="heartbeat.target.miss.nth"></xs:enumeration>
      <xs:enumeration value="flowtable.matched.fid"></xs:enumeration>
      <xs:enumeration value="flowtable.inport"></xs:enumeration>
      <xs:enumeration value="1"></xs:enumeration>
      <xs:enumeration value="packet.len"></xs:enumeration>
      <!--<xs:enumeration value="tunnel.outerlayer.ip.dsfield"></xs:enumeration>
      <xs:enumeration value="tunnel.innerlayer1.ip.dsfield"></xs:enumeration>
      <xs:enumeration value="tunnel.innerlayer2.ip.dsfield"></xs:enumeration>
      <xs:enumeration value="string"></xs:enumeration>
      <xs:enumeration value="hex"></xs:enumeration>
      <xs:enumeration value="radius"></xs:enumeration>
      <xs:enumeration value="sip"></xs:enumeration>
      <xs:enumeration value="SIP1"></xs:enumeration>
      <xs:enumeration value="SIP2"></xs:enumeration>
      <xs:enumeration value="SIP3"></xs:enumeration>
      <xs:enumeration value="SIP4"></xs:enumeration>-->
      <xs:enumeration value="quic.tag"></xs:enumeration>
      <xs:enumeration value="arp"></xs:enumeration>
      <xs:enumeration value="arp.request"></xs:enumeration>
      <xs:enumeration value="arp.request.sender.ip"></xs:enumeration>
      <xs:enumeration value="arp.request.target.ip"></xs:enumeration>
      <xs:enumeration value="arp.reply"></xs:enumeration>
      <xs:enumeration value="mec.mapping.ue.ipv4.connected"></xs:enumeration>
      <xs:enumeration value="vxlan"></xs:enumeration>
      <xs:enumeration value="vxlan.vni"></xs:enumeration>
    </xs:restriction>
  </xs:simpleType>

  <xs:simpleType name="comparisonType">
    <xs:restriction base="xs:string">
      <xs:enumeration value="=="></xs:enumeration>
      <xs:enumeration value="!="></xs:enumeration>
      <xs:enumeration value="&gt;="></xs:enumeration>
      <xs:enumeration value="&lt;="></xs:enumeration>
      <xs:enumeration value=""></xs:enumeration>
    </xs:restriction>
  </xs:simpleType>

  <xs:simpleType name="filesHandleType">
    <xs:restriction base="xs:string">
      <xs:enumeration value="delete"></xs:enumeration>
      <xs:enumeration value="move"></xs:enumeration>
    </xs:restriction>
  </xs:simpleType>

  <xs:group name="heartbeatGroup">
    <xs:sequence>
      <xs:element name="sendPort" type="xs:string" minOccurs="0" maxOccurs="1"></xs:element>
      <xs:element name="sendUDPPort" type="netPortType" minOccurs="0" maxOccurs="1"></xs:element>
      <xs:element name="sendInterval" type="xs:unsignedInt" minOccurs="0" maxOccurs="1"></xs:element>
      <xs:element name="receivePort" type="xs:string" minOccurs="0" maxOccurs="1"></xs:element>
      <xs:element name="receiveGWIP" type="ipType" minOccurs="0" maxOccurs="1"></xs:element>
      <xs:element name="receiveIP" type="ipType" minOccurs="0" maxOccurs="1"></xs:element>
      <xs:element name="receiveUDPPort" type="netPortType" minOccurs="0" maxOccurs="1"></xs:element>
      <xs:element name="receiveIntervalGreen" type="xs:unsignedInt" minOccurs="0" maxOccurs="1"></xs:element>
      <xs:element name="receiveIntervalRed" type="xs:unsignedInt" minOccurs="0" maxOccurs="1"></xs:element>
      <xs:element name="arpIP" type="ipType" minOccurs="0" maxOccurs="1"></xs:element>
    </xs:sequence>
  </xs:group>

  <xs:element name="action">
    <xs:complexType>
      <xs:choice>
        <xs:group ref="heartbeatGroup"></xs:group>
        <xs:sequence>
          <xs:element name="port">
            <xs:simpleType>
              <xs:restriction base="xs:string">
                <xs:pattern value="[A-Z][0-9]+"/>
              </xs:restriction>
            </xs:simpleType>
          </xs:element>
          <xs:element name="stripping" default="mpls" minOccurs="0" maxOccurs="unbounded">
            <xs:simpleType>
              <xs:restriction base="xs:string">
                <xs:enumeration value="mpls"></xs:enumeration>
                <xs:enumeration value="vlan"></xs:enumeration>
                <xs:enumeration value="grism"></xs:enumeration>
                <xs:enumeration value="gre-erspan"></xs:enumeration>
                <xs:enumeration value="gtp"></xs:enumeration>
                <xs:enumeration value="vxlan"></xs:enumeration>
                <xs:enumeration value="mpls-in-udp"></xs:enumeration>
                <xs:enumeration value="mpls-in-gre"></xs:enumeration>
                <xs:enumeration value="payload"></xs:enumeration>
              </xs:restriction>
            </xs:simpleType>
          </xs:element>
          <xs:element name="Q" minOccurs="0" maxOccurs="unbounded">
            <xs:complexType>
              <xs:simpleContent>
                <xs:extension base="vlanidType">
                  <xs:attribute name="type" type="vlanOpType" default="replace"></xs:attribute>
                </xs:extension>
              </xs:simpleContent>
            </xs:complexType>
          </xs:element>
          <xs:element name="QinQ" minOccurs="0" maxOccurs="unbounded">
            <xs:complexType>
              <xs:simpleContent>
                <xs:extension base="vlanidType">
                  <xs:attribute name="type" type="vlanOpType" default="add"></xs:attribute>
                </xs:extension>
              </xs:simpleContent>
            </xs:complexType>
          </xs:element>
          <xs:element name="maxlen" type="xs:unsignedInt" minOccurs="0" maxOccurs="1"></xs:element>
          <xs:element name="tagging" minOccurs="0" maxOccurs="unbounded">
            <xs:simpleType>
              <xs:restriction base="xs:string">
                <xs:enumeration value="grism"></xs:enumeration>
                <xs:enumeration value="timestamp"></xs:enumeration>
              </xs:restriction>
            </xs:simpleType>
          </xs:element>
        </xs:sequence>
        <xs:sequence>
          <xs:element name="portA" type="xs:string"></xs:element>
          <xs:element name="portB" type="xs:string"></xs:element>
        </xs:sequence>
      </xs:choice>
      <xs:attribute name="type">
        <xs:simpleType>
          <xs:restriction base="xs:string">
            <xs:enumeration value="heartbeat_udp"></xs:enumeration>
            <xs:enumeration value="arp_request"></xs:enumeration>
            <xs:enumeration value="arp_reply"></xs:enumeration>
            <xs:enumeration value="input-packet-stripping"></xs:enumeration>
            <xs:enumeration value="input-packet-vlanid"></xs:enumeration>
            <xs:enumeration value="input-packet-process"></xs:enumeration>
            <xs:enumeration value="linkpairs"></xs:enumeration>
          </xs:restriction>
        </xs:simpleType>
      </xs:attribute>
      <xs:attribute name="id" type="xs:integer" use="required"></xs:attribute>
      <xs:attribute name="name" type="xs:string"></xs:attribute>
      <xs:attribute name="alt" type="xs:string"></xs:attribute>
    </xs:complexType>
  </xs:element>

  <xs:element name="input">
    <xs:complexType>
      <xs:sequence>
        <xs:element name="port">
          <xs:simpleType>
            <xs:restriction base="xs:string">
              <xs:pattern value="[A-Z][0-9]+"/>
            </xs:restriction>
          </xs:simpleType>
        </xs:element>
        <xs:choice minOccurs="0" maxOccurs="unbounded">
          <xs:element name="filepath" type="xs:string"></xs:element>
          <xs:element name="time" type="xs:nonNegativeInteger" default="1"></xs:element>
          <xs:element name="sessionCompleteness" type="t1f0Type" default="1"></xs:element>
          <xs:element name="sorting" type="t1f0Type" default="0"></xs:element>
          <xs:element name="reverseSessionResult" type="t1f0Type" default="0"></xs:element>
          <xs:element name="msinterval" type="xs:nonNegativeInteger"></xs:element>
          <xs:element name="speed" type="xs:nonNegativeInteger"></xs:element>
          <xs:element name="scandir">
            <xs:complexType>
              <xs:simpleContent>
                <xs:extension base="xs:anyURI">
                  <xs:attribute name="interval" type="xs:nonNegativeInteger"></xs:attribute>
                  <xs:attribute name="minbytes" type="xs:nonNegativeInteger"></xs:attribute>
                  <xs:attribute name="timeout" type="xs:nonNegativeInteger"></xs:attribute>
                </xs:extension>
              </xs:simpleContent>
            </xs:complexType>
          </xs:element>
          <xs:element name="playedFilesHandle" type="filesHandleType"></xs:element>
          <xs:element name="playedFilesMoveTo" type="xs:anyURI"></xs:element>
          <xs:element name="protocol" type="xs:string"><!--
            <xs:simpleType>
              <xs:restriction base="xs:string">
                <xs:enumeration value="TCP"></xs:enumeration>
                <xs:enumeration value="UDP"></xs:enumeration>
              </xs:restriction>
            </xs:simpleType>-->
          </xs:element>
          <xs:element name="packet_size" type="xs:nonNegativeInteger"></xs:element>
          <xs:element name="packet_data" type="xs:string"></xs:element>
          <xs:element name="payload_text" type="xs:string"></xs:element>
          <xs:element name="src_mac" type="macType"></xs:element>
          <xs:element name="src_ip" type="ipType"></xs:element>
          <xs:element name="src_ip_min" type="ipType"></xs:element>
          <xs:element name="src_ip_max" type="ipType"></xs:element>
          <xs:element name="src_ip_inc" type="xs:integer"></xs:element>
          <xs:element name="src_ip_random" type="t1f0Type" default="0"></xs:element>
          <xs:element name="dest_mac" type="macType"></xs:element>
          <xs:element name="dest_ip" type="ipType"></xs:element>
          <xs:element name="dest_ip_min" type="ipType"></xs:element>
          <xs:element name="dest_ip_max" type="ipType"></xs:element>
          <xs:element name="dest_ip_inc" type="xs:integer"></xs:element>
          <xs:element name="dest_ip_random" type="t1f0Type" default="0"></xs:element>
          <xs:element name="src_port" type="netPortType"></xs:element>
          <xs:element name="src_port_min" type="netPortType"></xs:element>
          <xs:element name="src_port_max" type="netPortType"></xs:element>
          <xs:element name="src_port_inc" type="xs:integer"></xs:element>
          <xs:element name="src_port_random" type="t1f0Type" default="0"></xs:element>
          <xs:element name="dest_port" type="netPortType"></xs:element>
          <xs:element name="dest_port_min" type="netPortType"></xs:element>
          <xs:element name="dest_port_max" type="netPortType"></xs:element>
          <xs:element name="dest_port_inc" type="xs:integer"></xs:element>
          <xs:element name="dest_port_random" type="t1f0Type" default="0"></xs:element>
        </xs:choice>
      </xs:sequence>
      <xs:attribute name="id" type="xs:integer" use="optional"></xs:attribute>
      <xs:attribute name="name" type="xs:string"></xs:attribute>
      <xs:attribute name="type" default="replayPcap">
        <xs:simpleType>
          <xs:restriction base="xs:string">
            <xs:enumeration value="replayPcap"></xs:enumeration>
            <xs:enumeration value="refile"></xs:enumeration>
            <xs:enumeration value="traffic-gen"></xs:enumeration>
          </xs:restriction>
        </xs:simpleType>
      </xs:attribute>
      <xs:attribute name="alt" type="xs:string"></xs:attribute>
    </xs:complexType>
  </xs:element>
  <xs:element name="output">
    <xs:complexType>
      <xs:sequence>
        <xs:element name="port">
          <xs:simpleType>
            <xs:restriction base="xs:string">
              <xs:pattern value="[A-Z][0-9]+"/>
            </xs:restriction>
          </xs:simpleType>
        </xs:element>
        <xs:choice minOccurs="0" maxOccurs="unbounded">
          <xs:element name="gateway" type="ipType"></xs:element>
          <xs:element name="Q" type="vlanidType"></xs:element>
          <xs:element name="QinQ" type="vlanidType"></xs:element>
          <xs:element name="stripping" default="mpls">
            <xs:simpleType>
              <xs:restriction base="xs:string">
                <xs:enumeration value="payload"></xs:enumeration>
                <xs:enumeration value="vlan"></xs:enumeration>
                <xs:enumeration value="mpls"></xs:enumeration>
                <xs:enumeration value="gre"></xs:enumeration>
                <xs:enumeration value="vxlan"></xs:enumeration>
                <xs:enumeration value="mpls-in-udp"></xs:enumeration>
                <xs:enumeration value="mpls-in-gre"></xs:enumeration>
                <xs:enumeration value="gre-erspan"></xs:enumeration>
                <xs:enumeration value="gtp"></xs:enumeration>
                <xs:enumeration value="grism"></xs:enumeration>
              </xs:restriction>
            </xs:simpleType>
          </xs:element>
          <xs:element name="modify_srcip">
            <xs:complexType>
                <xs:simpleContent>
                  <xs:extension base="ipType">
                    <xs:attribute name="sessionDir" type="ynType" default="no" />
                    <xs:attribute name="nat" type="ynType" default="no" />
                  </xs:extension>
                </xs:simpleContent>
            </xs:complexType>
          </xs:element>
          <xs:element name="modify_dstip">
            <xs:complexType>
                <xs:simpleContent>
                  <xs:extension base="ipType">
                    <xs:attribute name="sessionDir" type="ynType" default="no" />
                  </xs:extension>
                </xs:simpleContent>
            </xs:complexType>
          </xs:element>
          <xs:element name="modify_srcmac" type="macType"></xs:element>
          <xs:element name="modify_dstmac" type="macType"></xs:element>
          <xs:element name="tagging">
            <xs:simpleType>
              <xs:restriction base="xs:string">
                <xs:enumeration value="timestamp"></xs:enumeration>
                <xs:enumeration value="gtp"></xs:enumeration>
                <xs:enumeration value="gtp2"></xs:enumeration>
                <xs:enumeration value="grism"></xs:enumeration>
                <xs:enumeration value="l2gre"></xs:enumeration>
                <xs:enumeration value="vxlan"></xs:enumeration>
              </xs:restriction>
            </xs:simpleType>
          </xs:element>
          <xs:element name="maxlen" type="xs:nonNegativeInteger"></xs:element>
          <!--<xs:element name="modify_srcip2mac" type="ipType" minOccurs="0" maxOccurs="1"></xs:element>
          <xs:element name="modify_dstip2mac" type="ipType" minOccurs="0" maxOccurs="1"></xs:element>-->
          <xs:element name="nvgre_dip" type="ipType"></xs:element>
          <xs:element name="nvgre_sip" type="ipType"></xs:element>
          <xs:element name="nvgre_dmac" type="macType"></xs:element>
          <xs:element name="nvgre_type">
            <xs:simpleType>
              <xs:restriction base="xs:string">
                <xs:enumeration value="eth"></xs:enumeration>
                <xs:enumeration value="ip"></xs:enumeration>
              </xs:restriction>
            </xs:simpleType>
          </xs:element>
          <xs:element name="arp_reply_target_mac" type="macType"></xs:element>
          <xs:element name="dns_response_ipv4">
            <xs:complexType>
                <xs:simpleContent>
                  <xs:extension base="ipType">
                    <xs:attribute name="noswapmac" type="ynType" />
                  </xs:extension>
                </xs:simpleContent>
            </xs:complexType>
          </xs:element>
          <xs:element name="dir">
            <xs:complexType>
                <xs:simpleContent>
                  <xs:extension base="xs:string">
                    <xs:attribute name="timeout" type="xs:nonNegativeInteger" default="0" />
                    <xs:attribute name="max_split_size" type="xs:unsignedLong" default="104857600" />
                    <xs:attribute name="category">
                      <xs:simpleType>
                        <xs:restriction base="xs:string">
                          <xs:enumeration value="month"></xs:enumeration>
                          <xs:enumeration value="day"></xs:enumeration>
                          <xs:enumeration value="hour"></xs:enumeration>
                          <xs:enumeration value="minute"></xs:enumeration>
                        </xs:restriction>
                      </xs:simpleType>
                    </xs:attribute>
                  </xs:extension>
                </xs:simpleContent>
            </xs:complexType>
          </xs:element>
          <xs:element name="dip" type="xs:string"></xs:element>
          <xs:element name="sport" type="netPortType"></xs:element>
          <xs:element name="dport" type="netPortType"></xs:element>
          <xs:element name="redirect2safeweb">
            <xs:complexType>
                <xs:simpleContent>
                  <xs:extension base="xs:string">
                    <xs:attribute name="noswapmac" type="ynType" />
                    <xs:attribute name="redirectPort" type="xs:string" />
                  </xs:extension>
                </xs:simpleContent>
            </xs:complexType>
          </xs:element>
          <xs:element name="arp_reply_default_mac">
            <xs:complexType>
            </xs:complexType>
          </xs:element>
          <xs:element name="modify_src_default_mac">
            <xs:complexType>
            </xs:complexType>
          </xs:element>
          <xs:element name="icmp_reply_fragment_need">
            <xs:complexType>
              <xs:attribute name="mtu" type="xs:unsignedShort" use="required"></xs:attribute>
            </xs:complexType>
          </xs:element>
          <xs:element name="dns_response_ipv6" type="ipv6Type"></xs:element>
          <xs:element name="modify_swapmac">
            <xs:complexType>
            </xs:complexType>
          </xs:element>
          <xs:element name="modify_tcp_syn_mss" type="xs:integer"></xs:element>
        </xs:choice>
      </xs:sequence>
      <xs:attribute name="id" type="xs:integer" use="required"></xs:attribute>
      <xs:attribute name="name" type="xs:string"></xs:attribute>
      <xs:attribute name="type">
        <xs:simpleType>
          <xs:restriction base="xs:string">
            <xs:enumeration value="mix"></xs:enumeration>
            <xs:enumeration value="httprequesthijack"></xs:enumeration>
            <xs:enumeration value="udpencap"></xs:enumeration>
            <xs:enumeration value="vlanid"></xs:enumeration>
            <xs:enumeration value="pcap"></xs:enumeration>
            <xs:enumeration value="stripping"></xs:enumeration>
            <xs:enumeration value="maxlen"></xs:enumeration>
            <xs:enumeration value="nvgre"></xs:enumeration>
          </xs:restriction>
        </xs:simpleType>
      </xs:attribute>
      <xs:attribute name="mtu" type="xs:nonNegativeInteger" default="0"></xs:attribute>
      <xs:attribute name="stl" type="xs:nonNegativeInteger" default="0"></xs:attribute>
      <xs:attribute name="arp_dstip_mac" type="ynType" default="no"></xs:attribute>
      <xs:attribute name="data-tag" type="xs:string"></xs:attribute>
      <xs:attribute name="data-index" type="xs:integer"></xs:attribute>
      <xs:attribute name="alt" type="xs:string"></xs:attribute>
      <xs:attribute name="minbps" type="xs:nonNegativeInteger"></xs:attribute>
      <xs:attribute name="maxbps" type="xs:nonNegativeInteger"></xs:attribute>
    </xs:complexType>
  </xs:element>

  <xs:element name="find">
    <xs:complexType>
      <xs:simpleContent>
        <xs:extension base="xs:string">
          <xs:attribute name="id" type="xs:integer" use="optional"></xs:attribute>
          <xs:attribute name="name" type="fieldType" default="regex"></xs:attribute>
          <xs:attribute name="relation" type="comparisonType" default=""></xs:attribute>
          <xs:attribute name="content" type="xs:string" default=""></xs:attribute>
          <xs:attribute name="n" type="fieldType" default="regex"></xs:attribute>
          <xs:attribute name="r" type="comparisonType" default=""></xs:attribute>
          <xs:attribute name="c" type="xs:string" default=""></xs:attribute>
        </xs:extension>
      </xs:simpleContent>
    </xs:complexType>
  </xs:element>
  <xs:element name="f" substitutionGroup="find"></xs:element>
  <xs:complexType name="criterion">
    <xs:sequence>
      <xs:element ref="find" minOccurs="0" maxOccurs="unbounded"></xs:element>
    </xs:sequence>
    <xs:attribute name="data-tag" type="xs:string"></xs:attribute>
  </xs:complexType>

  <xs:element name="filter">
    <xs:complexType>
      <xs:choice minOccurs="0">
        <xs:element name="or" type="criterion"></xs:element>
        <xs:element name="and" type="criterion"></xs:element>
      </xs:choice>
      <xs:attribute name="id" type="xs:integer" use="required"></xs:attribute>
      <xs:attribute name="name" type="xs:string"></xs:attribute>
      <xs:attribute name="sessionBase" type="ynType" default="yes"></xs:attribute>
      <!--<xs:attribute name="type" default="duplicate">
        <xs:simpleType>
          <xs:restriction base="xs:string">
            <xs:enumeration value="duplicate"></xs:enumeration>
            <xs:enumeration value="loadBalance"></xs:enumeration>
          </xs:restriction>
        </xs:simpleType>
      </xs:attribute>-->
      <xs:attribute name="masking" type="ynType" default="no"></xs:attribute>
      <xs:attribute name="maxPackets" type="xs:nonNegativeInteger" default="0"></xs:attribute>
      <xs:attribute name="matchedlog" type="ynType" default="no"></xs:attribute>
      <xs:attribute name="blockifempty" type="ynType" default="no"></xs:attribute>
      <xs:attribute name="tuple5_live_hashtable_size" type="xs:nonNegativeInteger"></xs:attribute>
      <xs:attribute name="alt" type="xs:string"></xs:attribute>
      <xs:attribute name="start">
        <xs:simpleType>
          <xs:restriction base="xs:string">
            <xs:enumeration value="l2"></xs:enumeration>
            <xs:enumeration value="l3"></xs:enumeration>
            <xs:enumeration value="l4"></xs:enumeration>
            <xs:enumeration value="l7"></xs:enumeration>
            <xs:enumeration value="http_body"></xs:enumeration>
          </xs:restriction>
        </xs:simpleType>
      </xs:attribute>
      <xs:attribute name="position" type="xs:nonNegativeInteger"></xs:attribute>
      <xs:attribute name="within" type="xs:nonNegativeInteger"></xs:attribute>
      <xs:attribute name="mpslog" type="xs:nonNegativeInteger"></xs:attribute>
    </xs:complexType>
  </xs:element>

  <xs:group name="nextGroup">
    <xs:sequence>
      <xs:element name="fid" minOccurs="0" maxOccurs="1">
        <xs:complexType>
          <xs:simpleContent>
            <xs:extension base="xs:string"><!--以逗號隔開，最多不能超過128-->
              <xs:attribute name="type" default="or">
                <xs:simpleType>
                  <xs:restriction base="xs:string">
                    <xs:enumeration value="or"></xs:enumeration>
                    <xs:enumeration value="and"></xs:enumeration>
                  </xs:restriction>
                </xs:simpleType>
              </xs:attribute>
              <xs:attribute name="alt" type="xs:string"></xs:attribute>
            </xs:extension>
          </xs:simpleContent>
        </xs:complexType>
      </xs:element>
      <xs:element name="out" minOccurs="0" maxOccurs="1">
        <xs:complexType>
          <xs:simpleContent>
            <xs:extension base="xs:string">
              <xs:attribute name="type" default="duplicate">
                <xs:simpleType>
                  <xs:restriction base="xs:string">
                    <xs:enumeration value="duplicate"></xs:enumeration>
                    <xs:enumeration value="loadBalance"></xs:enumeration>
                    <!--<xs:enumeration value="bypass"></xs:enumeration>-->
                  </xs:restriction>
                </xs:simpleType>
              </xs:attribute>
              <xs:attribute name="lbtype" default="session">
                <xs:simpleType>
                  <xs:restriction base="xs:string">
                    <xs:enumeration value="session"></xs:enumeration>
                    <xs:enumeration value="ethtype"></xs:enumeration>
                    <xs:enumeration value="iptype"></xs:enumeration>
                    <xs:enumeration value="smac"></xs:enumeration>
                    <xs:enumeration value="dmac"></xs:enumeration>
                    <xs:enumeration value="sip"></xs:enumeration>
                    <xs:enumeration value="dip"></xs:enumeration>
                    <xs:enumeration value="rr"></xs:enumeration>
                    <xs:enumeration value="5thash"></xs:enumeration>
                  </xs:restriction>
                </xs:simpleType>
              </xs:attribute>
              <xs:attribute name="failover" type="ynType" default="yes"></xs:attribute>
              <xs:attribute name="weight" type="xs:string" default="20,80"></xs:attribute>
              <!--<xs:attribute name="backup" type="xs:string"></xs:attribute>-->
              <xs:attribute name="alt" type="xs:string"></xs:attribute>
            </xs:extension>
          </xs:simpleContent>
        </xs:complexType>
      </xs:element>
      <xs:element name="next" minOccurs="0" maxOccurs="unbounded">
        <xs:complexType>
          <xs:group ref="nextGroup"></xs:group>
          <xs:attribute name="type" default="match">
            <xs:simpleType>
              <xs:restriction base="xs:string">
                <xs:enumeration value="match"></xs:enumeration>
                <xs:enumeration value="notmatch"></xs:enumeration>
              </xs:restriction>
            </xs:simpleType>
          </xs:attribute>
        </xs:complexType>
      </xs:element>
    </xs:sequence>
  </xs:group>

  <xs:element name="chain">
    <xs:complexType>
      <xs:sequence>
        <xs:element name="in" minOccurs="1" maxOccurs="1">
          <xs:complexType>
              <xs:simpleContent>
                <xs:extension base="xs:string">
                  <xs:attribute name="alt" type="xs:string"></xs:attribute>
                </xs:extension>
              </xs:simpleContent>
          </xs:complexType>
        </xs:element>
        <xs:group ref="nextGroup"></xs:group>
      </xs:sequence>
      <xs:attribute name="id" type="xs:integer" use="optional"></xs:attribute>
      <xs:attribute name="name" type="xs:string"></xs:attribute>
      <xs:attribute name="sessionUnique" type="ynType" default="no"></xs:attribute>
      <xs:attribute name="disable" type="ynType" default="no"></xs:attribute>
      <xs:attribute name="alt" type="xs:string"></xs:attribute>
    </xs:complexType>
  </xs:element>

  <xs:element name="bandwidthReserve">
    <xs:complexType>
      <xs:sequence>
        <xs:element name="rule" minOccurs="0" maxOccurs="unbounded">
          <xs:complexType>
            <xs:attribute name="id" type="xs:integer" use="required"></xs:attribute>
            <xs:attribute name="name" type="xs:string" use="required"></xs:attribute>
            <xs:attribute name="dir" use="required">
              <xs:simpleType>
                <xs:restriction base="xs:string">
                  <xs:enumeration value="in"></xs:enumeration>
                  <xs:enumeration value="out"></xs:enumeration>
                </xs:restriction>
              </xs:simpleType>
            </xs:attribute>
            <xs:attribute name="fid" type="xs:string" use="required"></xs:attribute>
            <xs:attribute name="minbps" type="xs:unsignedLong" use="required"></xs:attribute>
            <xs:attribute name="maxbps" type="xs:unsignedLong" use="required"></xs:attribute>
          </xs:complexType>
        </xs:element>
      </xs:sequence>
      <xs:attribute name="id" type="xs:integer" use="required"></xs:attribute>
      <xs:attribute name="name" type="xs:string" use="required"></xs:attribute>
      <xs:attribute name="port" type="xs:string" use="required"></xs:attribute>
      <xs:attribute name="maxinbps" type="xs:unsignedLong" use="required"></xs:attribute>
      <xs:attribute name="maxoutbps" type="xs:unsignedLong" use="required"></xs:attribute>
    </xs:complexType>
  </xs:element>

</xs:schema>

```
