# ARRAY NTB XML

## Tutorial

> ARRAY NTB XML is the standard markup language for SDN

> With ARRAY NTB XML you can define the Network with XML

> ARRAY NTB XML is easy to learn - You will enjoy it!

## Features

* Easy to learn and use, simple to edit
* User-Friendly
* Easy to integrate with third-party software
* Simple and lightweight

## Device Support

ARRAY, ARRAY-NETWORKS ([https://arraynetworks.com/](https://arraynetworks.com/))

## Online Doc

[https://arraynetworks.gitbook.io/array-xml/](https://arraynetworks.gitbook.io/array-xml/)

## A Simple ARRAY NTB XML

Define filter id=1 aka **F1** as black IP list and descript network topology in chain. When packets come from P0, if matched F1, send to P1, else if not matched, send to P2

```xml
<run>
    <filter id="1" name="black IP list">
        <or>
            <find name="ip.addr" relation="==" content="92.53.120.155" />
            <find name="ip.addr" relation="==" content="67.229.164.135" />
            <find name="ip.addr" relation="==" content="159.203.92.222" />            
        </or>
    </filter>
    <chain>
        <in>P0</in>
        <fid>F1</fid>
        <out>P1</out>
        <next type="notmatch">
            <out>P2</out>
        </next>
    </chain>
</run>
```
## NTB Training Material

ARRAY-NETWORKS NTB Training ([https://arraynetworks.gitbook.io/array-ntb-training/](https://arraynetworks.gitbook.io/array-ntb-training/))

## Version

5.3.230719
