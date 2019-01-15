# ICMP Protocol

## ICMP Header Format

```
+--------+--------+----------------+
|  Type  |  Code  | Header CheckSum|
+--------+-------------------------+
|  Identifier     | Sequence Number|
+-----------------+----------------+
|           Data                   |
+----------------------------------+
```

- Type: 8 bits, 0(echo reply message) 1(echo request message)
- Code: 8 bits. Cleared to 0
- ICMP Header Checksum: 16 bits
- Identifier: 16 bits(process id in linux)
- Sequence number: 16 bits

Ethernet frame format:

```
+-----------------------------------+------+
|MAC header| IP header| ICMP header | Data |
+-----------------------------------+------+
```
Max MTU(`1518`bytes): `18`bytes(MAC header) + `20`bytes(IP header) + `8`bytes(ICMP header) + Data size

so, maximum data size is: `1472`bytes

## PING

MAC:

```
$ping -D -s 1472 114.114.114.114
```

Linux:

```
$ping -M dont -s 1472 114.114.114.114
```
- `-s` Specify the number of data bytes to be sent, default is 56,1472 + 8(ICMP Header) + 20(IP Header) = 1500(MTU  )
- `-D` Set the Don't Fragment bit

## ICMP Analysis

### Operation

```
$ping -D -s 1472 114.114.114.114
```

packet capture:

```
#tcpdump host 114.114.114.114 -x
```

### ICMP Request

#### Request Example

```
09:03:25.454198 IP 30.43.84.5 > public1.114dns.com: ICMP echo request, id 38790, seq 41, length 1480
	0x0000:  0074 9c94 5d18 88e9 fe7b 3023 0800 4500
	0x0010:  05dc e419 4000 4001 f9f2 1e2b 5405 7272
	0x0020:  7272 0800 6b4a 9786 0029 5c3d 315d 0006
	0x0030:  ee12 0809 0a0b 0c0d 0e0f 1011 1213 1415
	0x0040:  1617 1819 1a1b 1c1d 1e1f 2021 2223 2425
	0x0050:  2627 2829 2a2b 2c2d 2e2f 3031 3233 3435
	0x0060:  3637 3839 3a3b 3c3d 3e3f 4041 4243 4445
	0x0070:  4647 4849 4a4b 4c4d 4e4f 5051 5253 5455
	0x0080:  5657 5859 5a5b 5c5d 5e5f 6061 6263 6465
	0x0090:  6667 6869 6a6b 6c6d 6e6f 7071 7273 7475
	0x00a0:  7677 7879 7a7b 7c7d 7e7f 8081 8283 8485
	0x00b0:  8687 8889 8a8b 8c8d 8e8f 9091 9293 9495
	0x00c0:  9697 9899 9a9b 9c9d 9e9f a0a1 a2a3 a4a5
	0x00d0:  a6a7 a8a9 aaab acad aeaf b0b1 b2b3 b4b5
	0x00e0:  b6b7 b8b9 babb bcbd bebf c0c1 c2c3 c4c5
	0x00f0:  c6c7 c8c9 cacb cccd cecf d0d1 d2d3 d4d5
	0x0100:  d6d7 d8d9 dadb dcdd dedf e0e1 e2e3 e4e5
	0x0110:  e6e7 e8e9 eaeb eced eeef f0f1 f2f3 f4f5
	0x0120:  f6f7 f8f9 fafb fcfd feff 0001 0203 0405
	0x0130:  0607 0809 0a0b 0c0d 0e0f 1011 1213 1415
	0x0140:  1617 1819 1a1b 1c1d 1e1f 2021 2223 2425
	0x0150:  2627 2829 2a2b 2c2d 2e2f 3031 3233 3435
	0x0160:  3637 3839 3a3b 3c3d 3e3f 4041 4243 4445
	0x0170:  4647 4849 4a4b 4c4d 4e4f 5051 5253 5455
	0x0180:  5657 5859 5a5b 5c5d 5e5f 6061 6263 6465
	0x0190:  6667 6869 6a6b 6c6d 6e6f 7071 7273 7475
	0x01a0:  7677 7879 7a7b 7c7d 7e7f 8081 8283 8485
	0x01b0:  8687 8889 8a8b 8c8d 8e8f 9091 9293 9495
	0x01c0:  9697 9899 9a9b 9c9d 9e9f a0a1 a2a3 a4a5
	0x01d0:  a6a7 a8a9 aaab acad aeaf b0b1 b2b3 b4b5
	0x01e0:  b6b7 b8b9 babb bcbd bebf c0c1 c2c3 c4c5
	0x01f0:  c6c7 c8c9 cacb cccd cecf d0d1 d2d3 d4d5
	0x0200:  d6d7 d8d9 dadb dcdd dedf e0e1 e2e3 e4e5
	0x0210:  e6e7 e8e9 eaeb eced eeef f0f1 f2f3 f4f5
	0x0220:  f6f7 f8f9 fafb fcfd feff 0001 0203 0405
	0x0230:  0607 0809 0a0b 0c0d 0e0f 1011 1213 1415
	0x0240:  1617 1819 1a1b 1c1d 1e1f 2021 2223 2425
	0x0250:  2627 2829 2a2b 2c2d 2e2f 3031 3233 3435
	0x0260:  3637 3839 3a3b 3c3d 3e3f 4041 4243 4445
	0x0270:  4647 4849 4a4b 4c4d 4e4f 5051 5253 5455
	0x0280:  5657 5859 5a5b 5c5d 5e5f 6061 6263 6465
	0x0290:  6667 6869 6a6b 6c6d 6e6f 7071 7273 7475
	0x02a0:  7677 7879 7a7b 7c7d 7e7f 8081 8283 8485
	0x02b0:  8687 8889 8a8b 8c8d 8e8f 9091 9293 9495
	0x02c0:  9697 9899 9a9b 9c9d 9e9f a0a1 a2a3 a4a5
	0x02d0:  a6a7 a8a9 aaab acad aeaf b0b1 b2b3 b4b5
	0x02e0:  b6b7 b8b9 babb bcbd bebf c0c1 c2c3 c4c5
	0x02f0:  c6c7 c8c9 cacb cccd cecf d0d1 d2d3 d4d5
	0x0300:  d6d7 d8d9 dadb dcdd dedf e0e1 e2e3 e4e5
	0x0310:  e6e7 e8e9 eaeb eced eeef f0f1 f2f3 f4f5
	0x0320:  f6f7 f8f9 fafb fcfd feff 0001 0203 0405
	0x0330:  0607 0809 0a0b 0c0d 0e0f 1011 1213 1415
	0x0340:  1617 1819 1a1b 1c1d 1e1f 2021 2223 2425
	0x0350:  2627 2829 2a2b 2c2d 2e2f 3031 3233 3435
	0x0360:  3637 3839 3a3b 3c3d 3e3f 4041 4243 4445
	0x0370:  4647 4849 4a4b 4c4d 4e4f 5051 5253 5455
	0x0380:  5657 5859 5a5b 5c5d 5e5f 6061 6263 6465
	0x0390:  6667 6869 6a6b 6c6d 6e6f 7071 7273 7475
	0x03a0:  7677 7879 7a7b 7c7d 7e7f 8081 8283 8485
	0x03b0:  8687 8889 8a8b 8c8d 8e8f 9091 9293 9495
	0x03c0:  9697 9899 9a9b 9c9d 9e9f a0a1 a2a3 a4a5
	0x03d0:  a6a7 a8a9 aaab acad aeaf b0b1 b2b3 b4b5
	0x03e0:  b6b7 b8b9 babb bcbd bebf c0c1 c2c3 c4c5
	0x03f0:  c6c7 c8c9 cacb cccd cecf d0d1 d2d3 d4d5
	0x0400:  d6d7 d8d9 dadb dcdd dedf e0e1 e2e3 e4e5
	0x0410:  e6e7 e8e9 eaeb eced eeef f0f1 f2f3 f4f5
	0x0420:  f6f7 f8f9 fafb fcfd feff 0001 0203 0405
	0x0430:  0607 0809 0a0b 0c0d 0e0f 1011 1213 1415
	0x0440:  1617 1819 1a1b 1c1d 1e1f 2021 2223 2425
	0x0450:  2627 2829 2a2b 2c2d 2e2f 3031 3233 3435
	0x0460:  3637 3839 3a3b 3c3d 3e3f 4041 4243 4445
	0x0470:  4647 4849 4a4b 4c4d 4e4f 5051 5253 5455
	0x0480:  5657 5859 5a5b 5c5d 5e5f 6061 6263 6465
	0x0490:  6667 6869 6a6b 6c6d 6e6f 7071 7273 7475
	0x04a0:  7677 7879 7a7b 7c7d 7e7f 8081 8283 8485
	0x04b0:  8687 8889 8a8b 8c8d 8e8f 9091 9293 9495
	0x04c0:  9697 9899 9a9b 9c9d 9e9f a0a1 a2a3 a4a5
	0x04d0:  a6a7 a8a9 aaab acad aeaf b0b1 b2b3 b4b5
	0x04e0:  b6b7 b8b9 babb bcbd bebf c0c1 c2c3 c4c5
	0x04f0:  c6c7 c8c9 cacb cccd cecf d0d1 d2d3 d4d5
	0x0500:  d6d7 d8d9 dadb dcdd dedf e0e1 e2e3 e4e5
	0x0510:  e6e7 e8e9 eaeb eced eeef f0f1 f2f3 f4f5
	0x0520:  f6f7 f8f9 fafb fcfd feff 0001 0203 0405
	0x0530:  0607 0809 0a0b 0c0d 0e0f 1011 1213 1415
	0x0540:  1617 1819 1a1b 1c1d 1e1f 2021 2223 2425
	0x0550:  2627 2829 2a2b 2c2d 2e2f 3031 3233 3435
	0x0560:  3637 3839 3a3b 3c3d 3e3f 4041 4243 4445
	0x0570:  4647 4849 4a4b 4c4d 4e4f 5051 5253 5455
	0x0580:  5657 5859 5a5b 5c5d 5e5f 6061 6263 6465
	0x0590:  6667 6869 6a6b 6c6d 6e6f 7071 7273 7475
	0x05a0:  7677 7879 7a7b 7c7d 7e7f 8081 8283 8485
	0x05b0:  8687 8889 8a8b 8c8d 8e8f 9091 9293 9495
	0x05c0:  9697 9899 9a9b 9c9d 9e9f a0a1 a2a3 a4a5
	0x05d0:  a6a7 a8a9 aaab acad aeaf b0b1 b2b3 b4b5
	0x05e0:  b6b7 b8b9 babb bcbd bebf
```


#### MAC header:

```
0074 9c94 5d18 88e9 fe7b 3023 0800
```
#### IP header:

```
4500 05dc e419 4000 4001 f9f2 1e2b 5405 7272 7272
```

#### ICMP header:

```
0800 6b4a 9786 0029
```

- Type: 08, icmp request
- Code: 00
- Checksum: 6b4a
- Identifier: 9786
- Sequence Numer: 0029

### ICMP Response

#### ICMP Response Example

```
09:03:25.467441 IP public1.114dns.com > 30.43.84.5: ICMP echo reply, id 38790, seq 41, length 1480
	0x0000:  88e9 fe7b 3023 0074 9c94 5d18 0800 4500
	0x0010:  05dc f295 0000 3601 3577 7272 7272 1e2b
	0x0020:  5405 0000 734a 9786 0029 5c3d 315d 0006
	0x0030:  ee12 0809 0a0b 0c0d 0e0f 1011 1213 1415
	0x0040:  1617 1819 1a1b 1c1d 1e1f 2021 2223 2425
	0x0050:  2627 2829 2a2b 2c2d 2e2f 3031 3233 3435
	0x0060:  3637 3839 3a3b 3c3d 3e3f 4041 4243 4445
	0x0070:  4647 4849 4a4b 4c4d 4e4f 5051 5253 5455
	0x0080:  5657 5859 5a5b 5c5d 5e5f 6061 6263 6465
	0x0090:  6667 6869 6a6b 6c6d 6e6f 7071 7273 7475
	0x00a0:  7677 7879 7a7b 7c7d 7e7f 8081 8283 8485
	0x00b0:  8687 8889 8a8b 8c8d 8e8f 9091 9293 9495
	0x00c0:  9697 9899 9a9b 9c9d 9e9f a0a1 a2a3 a4a5
	0x00d0:  a6a7 a8a9 aaab acad aeaf b0b1 b2b3 b4b5
	0x00e0:  b6b7 b8b9 babb bcbd bebf c0c1 c2c3 c4c5
	0x00f0:  c6c7 c8c9 cacb cccd cecf d0d1 d2d3 d4d5
	0x0100:  d6d7 d8d9 dadb dcdd dedf e0e1 e2e3 e4e5
	0x0110:  e6e7 e8e9 eaeb eced eeef f0f1 f2f3 f4f5
	0x0120:  f6f7 f8f9 fafb fcfd feff 0001 0203 0405
	0x0130:  0607 0809 0a0b 0c0d 0e0f 1011 1213 1415
	0x0140:  1617 1819 1a1b 1c1d 1e1f 2021 2223 2425
	0x0150:  2627 2829 2a2b 2c2d 2e2f 3031 3233 3435
	0x0160:  3637 3839 3a3b 3c3d 3e3f 4041 4243 4445
	0x0170:  4647 4849 4a4b 4c4d 4e4f 5051 5253 5455
	0x0180:  5657 5859 5a5b 5c5d 5e5f 6061 6263 6465
	0x0190:  6667 6869 6a6b 6c6d 6e6f 7071 7273 7475
	0x01a0:  7677 7879 7a7b 7c7d 7e7f 8081 8283 8485
	0x01b0:  8687 8889 8a8b 8c8d 8e8f 9091 9293 9495
	0x01c0:  9697 9899 9a9b 9c9d 9e9f a0a1 a2a3 a4a5
	0x01d0:  a6a7 a8a9 aaab acad aeaf b0b1 b2b3 b4b5
	0x01e0:  b6b7 b8b9 babb bcbd bebf c0c1 c2c3 c4c5
	0x01f0:  c6c7 c8c9 cacb cccd cecf d0d1 d2d3 d4d5
	0x0200:  d6d7 d8d9 dadb dcdd dedf e0e1 e2e3 e4e5
	0x0210:  e6e7 e8e9 eaeb eced eeef f0f1 f2f3 f4f5
	0x0220:  f6f7 f8f9 fafb fcfd feff 0001 0203 0405
	0x0230:  0607 0809 0a0b 0c0d 0e0f 1011 1213 1415
	0x0240:  1617 1819 1a1b 1c1d 1e1f 2021 2223 2425
	0x0250:  2627 2829 2a2b 2c2d 2e2f 3031 3233 3435
	0x0260:  3637 3839 3a3b 3c3d 3e3f 4041 4243 4445
	0x0270:  4647 4849 4a4b 4c4d 4e4f 5051 5253 5455
	0x0280:  5657 5859 5a5b 5c5d 5e5f 6061 6263 6465
	0x0290:  6667 6869 6a6b 6c6d 6e6f 7071 7273 7475
	0x02a0:  7677 7879 7a7b 7c7d 7e7f 8081 8283 8485
	0x02b0:  8687 8889 8a8b 8c8d 8e8f 9091 9293 9495
	0x02c0:  9697 9899 9a9b 9c9d 9e9f a0a1 a2a3 a4a5
	0x02d0:  a6a7 a8a9 aaab acad aeaf b0b1 b2b3 b4b5
	0x02e0:  b6b7 b8b9 babb bcbd bebf c0c1 c2c3 c4c5
	0x02f0:  c6c7 c8c9 cacb cccd cecf d0d1 d2d3 d4d5
	0x0300:  d6d7 d8d9 dadb dcdd dedf e0e1 e2e3 e4e5
	0x0310:  e6e7 e8e9 eaeb eced eeef f0f1 f2f3 f4f5
	0x0320:  f6f7 f8f9 fafb fcfd feff 0001 0203 0405
	0x0330:  0607 0809 0a0b 0c0d 0e0f 1011 1213 1415
	0x0340:  1617 1819 1a1b 1c1d 1e1f 2021 2223 2425
	0x0350:  2627 2829 2a2b 2c2d 2e2f 3031 3233 3435
	0x0360:  3637 3839 3a3b 3c3d 3e3f 4041 4243 4445
	0x0370:  4647 4849 4a4b 4c4d 4e4f 5051 5253 5455
	0x0380:  5657 5859 5a5b 5c5d 5e5f 6061 6263 6465
	0x0390:  6667 6869 6a6b 6c6d 6e6f 7071 7273 7475
	0x03a0:  7677 7879 7a7b 7c7d 7e7f 8081 8283 8485
	0x03b0:  8687 8889 8a8b 8c8d 8e8f 9091 9293 9495
	0x03c0:  9697 9899 9a9b 9c9d 9e9f a0a1 a2a3 a4a5
	0x03d0:  a6a7 a8a9 aaab acad aeaf b0b1 b2b3 b4b5
	0x03e0:  b6b7 b8b9 babb bcbd bebf c0c1 c2c3 c4c5
	0x03f0:  c6c7 c8c9 cacb cccd cecf d0d1 d2d3 d4d5
	0x0400:  d6d7 d8d9 dadb dcdd dedf e0e1 e2e3 e4e5
	0x0410:  e6e7 e8e9 eaeb eced eeef f0f1 f2f3 f4f5
	0x0420:  f6f7 f8f9 fafb fcfd feff 0001 0203 0405
	0x0430:  0607 0809 0a0b 0c0d 0e0f 1011 1213 1415
	0x0440:  1617 1819 1a1b 1c1d 1e1f 2021 2223 2425
	0x0450:  2627 2829 2a2b 2c2d 2e2f 3031 3233 3435
	0x0460:  3637 3839 3a3b 3c3d 3e3f 4041 4243 4445
	0x0470:  4647 4849 4a4b 4c4d 4e4f 5051 5253 5455
	0x0480:  5657 5859 5a5b 5c5d 5e5f 6061 6263 6465
	0x0490:  6667 6869 6a6b 6c6d 6e6f 7071 7273 7475
	0x04a0:  7677 7879 7a7b 7c7d 7e7f 8081 8283 8485
	0x04b0:  8687 8889 8a8b 8c8d 8e8f 9091 9293 9495
	0x04c0:  9697 9899 9a9b 9c9d 9e9f a0a1 a2a3 a4a5
	0x04d0:  a6a7 a8a9 aaab acad aeaf b0b1 b2b3 b4b5
	0x04e0:  b6b7 b8b9 babb bcbd bebf c0c1 c2c3 c4c5
	0x04f0:  c6c7 c8c9 cacb cccd cecf d0d1 d2d3 d4d5
	0x0500:  d6d7 d8d9 dadb dcdd dedf e0e1 e2e3 e4e5
	0x0510:  e6e7 e8e9 eaeb eced eeef f0f1 f2f3 f4f5
	0x0520:  f6f7 f8f9 fafb fcfd feff 0001 0203 0405
	0x0530:  0607 0809 0a0b 0c0d 0e0f 1011 1213 1415
	0x0540:  1617 1819 1a1b 1c1d 1e1f 2021 2223 2425
	0x0550:  2627 2829 2a2b 2c2d 2e2f 3031 3233 3435
	0x0560:  3637 3839 3a3b 3c3d 3e3f 4041 4243 4445
	0x0570:  4647 4849 4a4b 4c4d 4e4f 5051 5253 5455
	0x0580:  5657 5859 5a5b 5c5d 5e5f 6061 6263 6465
	0x0590:  6667 6869 6a6b 6c6d 6e6f 7071 7273 7475
	0x05a0:  7677 7879 7a7b 7c7d 7e7f 8081 8283 8485
	0x05b0:  8687 8889 8a8b 8c8d 8e8f 9091 9293 9495
	0x05c0:  9697 9899 9a9b 9c9d 9e9f a0a1 a2a3 a4a5
	0x05d0:  a6a7 a8a9 aaab acad aeaf b0b1 b2b3 b4b5
	0x05e0:  b6b7 b8b9 babb bcbd bebf
```

#### MAC header

```
88e9 fe7b 3023 0074 9c94 5d18 0800
```

#### IP header

```
4500 05dc f295 0000 3601 3577 7272 7272 1e2b 5405
```

#### ICMP header

```
0000 734a 9786 0029
```
- Type: 00, icmp reply
- Code: 00
- Checksum: 734a
- Identifier: 9786(same as icmp request)
- Sequence Number: 0029(same as icmp request)

## Reference

1. [http://www.networksorcery.com/enp/rfc/rfc792.txt](http://www.networksorcery.com/enp/rfc/rfc792.txt)
2. [http://www.networksorcery.com/enp/protocol/icmp/msg8.htm](http://www.networksorcery.com/enp/protocol/icmp/msg8.htm)
