# Eddystone-URL

The Eddystone-URL frame broadcasts a URL using a compressed encoding format in order to fit more within the limited advertisement packet.

Once decoded, the URL can be used by any client with access to the internet.  For example, if an Eddystone-URL beacon were to broadcast the URL `https://goo.gl/Aq18zF`, then any client that received this packet could choose to [visit that url](https://goo.gl/Aq18zF).

The Eddystone-URL frame forms the backbone of the [Physical Web](http://physical-web.org), an effort to enable frictionless discovery of web content relating to one’s surroundings. Eddystone-URL incorporates all of the learnings from the [UriBeacon](http://uribeacon.org) format from which it evolved.

## Frame Specification

Byte offset | Field | Description
------------|-------|------------
0 | Frame Type | Value = `0x10`
1 | TX Power | Calibrated Tx power at 0 m
2 | URL Scheme | Encoded Scheme Prefix
3+ | Encoded URL | Length 0-11

### Tx Power Level

Tx power is the received power at 0 meters, in dBm, and the value ranges from -100 dBm to +20 dBm to a resolution of 1 dBm.

Note to developers: the best way to determine the precise value to put into this field is to measure the actual output of your beacon from 1 meter away and then add 41dBm to that. 41dBm is the signal loss that occurs over 1 meter.

The value is a signed 8 bit integer as specified by
[TX Power Level Bluetooth Characteristic](https://developer.bluetooth.org/gatt/characteristics/Pages/CharacteristicViewer.aspx?u=org.bluetooth.characteristic.tx_power_level.xml).

#### Examples

* The value 0x12 is interpreted as +18dBm
* The value 0xEE is interpreted as -18dBm

### URL Scheme 

The URL Scheme byte defines the identifier scheme and it is a direct correspondence for multiple URL shorteners
**To Standardize URL formats all URL's used will be shortened by any URL shortener. The following table lists some popular ones and more can be added in the future
Doing this saves us a lot of payload bytes at the cost of loss of generality. It enables us to work with really low payload capable devices like the nrf24L01
Decimal  | Hex        | Expansion
:------- | :--------- | :--------
0        | 0x00       | `https://goo.gl/`
1        | 0x01       | `http://tinyurl.com/`
2        | 0x02       | `http://ow.ly/`

### Eddystone-URL HTTP URL encoding

The HTTP URL scheme is defined by RFC 1738, for example
`https://goo.gl/S6zT6P`, and is used to designate Internet resources
accessible using HTTP (HyperText Transfer Protocol).

After receiving the above byte it must be appended by the US-ASCII valid URL characters that are alloted by the URL shortening engine.

|Decimal  | Hex        | Expansion
|:------- | :--------- | :--------
|48        | 0x30       | `0`
|49        | 0x31       | `1`
|..        | ..        | ..
|122      | 0x7A       | `Z`

Note: 
* URLs are written only with the graphic printable characters of the US-ASCII coded character set. The octets **00-20** and **7F-FF** hexadecimal are not used. See “Excluded US-ASCII Characters” in RFC 2936.
* Range **07-13** define the same top level domains as **00-06** without a /.


