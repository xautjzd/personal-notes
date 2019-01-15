# TCP Protocol

## TCP Header Format

```
 0                   1                   2                   3
    0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |          Source Port          |       Destination Port        |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                        Sequence Number                        |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                    Acknowledgment Number                      |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |  Data |           |U|A|P|R|S|F|                               |
   | Offset| Reserved  |R|C|S|S|Y|I|            Window             |
   |       |           |G|K|H|T|N|N|                               |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |           Checksum            |         Urgent Pointer        |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                    Options                    |    Padding    |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                             data                              |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

- Source Port: `16` bits
- Destination Port: `16` bits
- Sequence Number: `32` bits
- Acknowledgment Number: `32` bits
- Data Offset: `4` bits. The number of 32 bit words in the TCP Header. Maximum header size is `60` bytes(2^4 - 1) * 4
- Reserved: `6` bits. Reserved for future use. Must be zero
- Flag: `6` bits.
- Window: `16` bits. The number of bytes the sender will accept
- Checksum: `16` bits
- Urgent Pointer: `16` bits
- Options: variable

Options may occupy space at the end of the TCP header and are a
multiple of 8 bits in length.  All options are included in the
checksum.  An option may begin on any octet boundary.  There are two
cases for the format of an option:

   Case 1:  A single octet of option-kind.

   Case 2:  An octet of option-kind, an octet of option-length, and
            the actual option-data octets.

The option-length counts the two octets of option-kind and
option-length as well as the option-data octets.

TCP Options:

```
+------+--------+------+---------+--------------------------+
| Kind | Length | Name | RFC     |   Description            |
+-----------------------------------------------------------+
|  0   |   1    | EOL  | RFC 793 | End of Option List       |
+-----------------------------------------------------------+
|  1   |   1    | NOP  | RFC 793 | No Option                |
+-----------------------------------------------------------+
|  2   |   4    | MSS  | RFC 793 | Maximum Size Segment     |
+-----------------------------------------------------------+
|  3   |   3    | WSOPT| RFC 1323| TCP Window Scale Option  |
+-----------------------------------------------------------+
|  4   |   2    |      | RFC 2018| Sack Permitted Option    |
+-----------------------------------------------------------+
|  5   |Variable| SACK | RFC 2018| SACK Option              |
+-----------------------------------------------------------+
|  8   |   10   | TSPOT| RFC 1323| TCP Timestamps Option    |
+------+--------+-------------------------------------------+
```

## TCP Three-Way HandShake Example

### How to to?

```
#tcpdump host 42.121.252.58 -x
```

first packet:

```
13:04:02.303604 IP 192.168.31.100.52322 > 42.121.252.58.http: Flags [S], seq 2136354314, win 65535, options [mss 1460,nop,wscale 6,nop,nop,TS val 1861081185 ecr 0,sackOK,eol], length 0
	0x0000:  f0b4 296e d90f 88e9 fe7b 3023 0800 4500
	0x0010:  0040 0000 4000 4006 33f8 c0a8 1f64 2a79
	0x0020:  fc3a cc62 0050 7f56 2e0a 0000 0000 b002
	0x0030:  ffff 6ed9 0000 0204 05b4 0103 0306 0101
	0x0040:  080a 6eed d861 0000 0000 0402 0000
```

Ethernet Header:

- Destination mac address: `f0b4 296e d90f`
- Source mac address: `88e9 fe7b 3023`
- Protocol Type: `0800`(IP)

IP Header:

- Version: `4`bit(4: ipv4, 6:ipv6)
- IHL: `5` * 32bit
- Type of Service: `00`
- Total Length: `0040`(64 bytes)
- Identification: `0000`
- Flags: `4`(0100)
- Fragment Offset: `000`(include last bit of Flags)
- Time to Live: `40`(56)
- Protocol: `06`(ICMP:0x01, TCP: 0x06, UDP:0x11)
- Header Checksum: `33f8`
- Source Address: `c0a8 1f64`(192.168.31)
- Destination Address: `2a79 fc3a`(42.121.252.58)

TCP Header:

- Source Port: `cc62`(52322)
- Destination Port: `0050`(80)
- Sequence Number: `7f56 2e0a`
- Acknowledgment Number: `0000 0000`
- Data Offset:`b`(11 * 4)
- Reserved: 6bit
- Flag: `02`(6bit, SYN = 1)
- Window: `ffff`(65535)
- Checksum: `6ed9`
- Urgent Pointer: `0000` 
- Options: `0204 05b4 0103 0306 0101 080a 6eed d861 0000 0000 0402 0000`



second packet:

```
13:04:02.315207 IP 42.121.252.58.http > 192.168.31.100.52322: Flags [S.], seq 3556668287, ack 2136354315, win 14600, options [mss 1444,nop,nop,sackOK,nop,wscale 9], length 0
	0x0000:  88e9 fe7b 3023 f0b4 296e d90f 0800 4500
	0x0010:  0034 0000 4000 2c06 4804 2a79 fc3a c0a8
	0x0020:  1f64 0050 cc62 d3fe 737f 7f56 2e0b 8012
	0x0030:  3908 6db4 0000 0204 05a4 0101 0402 0103
	0x0040:  0309
```

TCP:

- Source Port: `0050`
- Destination Port: `cc62`
- Sequence Number: `d3fe 737f`
- Acknowledgment Number: `7f56 2e0b`
- Data Offset: 8(8 * 4 bytes)
- Flag: 12(010010, SYN = 1, ACK = 1)
- Window: 3908
- Checksum: `6db4`
- Urgent Pointer: `0000`
- Options: `0204 05a4 0101 0402 0103 0309` 
third packet:

```
13:04:02.315226 IP 192.168.31.100.52322 > 42.121.252.58.http: Flags [.], ack 1, win 4096, length 0
	0x0000:  f0b4 296e d90f 88e9 fe7b 3023 0800 4500
	0x0010:  0028 0000 4000 4006 3410 c0a8 1f64 2a79
	0x0020:  fc3a cc62 0050 7f56 2e0b d3fe 7380 5010
	0x0030:  1000 d780 0000
```

TODO

- Options解释

## 参考资料: 

1. [https://tools.ietf.org/html/rfc793#section-3.1](https://tools.ietf.org/html/rfc793#section-3.1)
2. [https://tools.ietf.org/html/rfc1323](https://tools.ietf.org/html/rfc1323)
3. [https://tools.ietf.org/html/rfc2018](https://tools.ietf.org/html/rfc2018)
