# MTU, MSS 이야기
### MTU(Maximum Transmission Unit)
- 네트워크에서 한 번에 보낼 수 있는 최대 데이터 크기(헤더 포함)
- 단위는 Outet(옥텟)이며 Byte와 같은 사이즈(1 Outet = 1 Byte)
- 일반적인 이더넷에서 수용할 수 있는 크기는 1,500바이트
- MTU는 2계층의 데이터값

### MSS(Maximum Segment Size)
- MSS는 4계층에서 가질 수 있는 최대 데이터 값
- 순수하게 MTU에서 각종 헤더를 제외한 값
- 일반적인 IP 표준 헤더 20바이트
- 일반적인 TCP 표준 헤더 20바이트(추가되는 옵션 헤더 제외)
- 일반적인 이더넷인 경우 MSS 값은 1,460바이트(1500 - 20(IP) - 20(TCP))

### Header 크기
```
-   TCP Header: 20 Bytes(옵션 헤더 제외)
-   UDP Header:  8 Bytes
-    IP Header: 20 Bytes
-  ICMP Header:  8 Bytes
- IPSec Header: 가변적이며 최대 64 Bytes
```

#### IPSec Header
- 전송모드와 터널 모드에 따라 오버헤드가 다름
- 암호화/인증 알고리즘과 HMAC에 따라 오버헤드가 다름
- 터널 모드에서 2개의 IP 헤더가 전송됨. (내부, 외부)

- ESP Overhead
```
  - Outer IP Header:        20 Bytes
  - Sequence Number:         4 Bytes
  - SPI:                     4 Bytes
  - Initialization Vector: 8 or 16 Bytes(DES or AES)
  - ESP Padding:         [0-15]Bytes
  - Padding Length:          1 Bytes
  - Next Header:             1 Bytes
  - Authentication Data: 12-32 Bytes(SHA-1 to SHA-512)
  - Total:              [50-93]Bytes
```
- Initialization Vector
```
  - AES: 16 Bytes
  - DES:  8 Bytes
```
- Authentication Data
```
  - MD5/SHA-1: 12 Bytes
  - SHA-256:   16 Bytes
  - SHA-384:   24 Bytes
  - SHA-512:   32 Bytes
```

### MTU 문제를 진단하는 빠르고 쉬운 방법
- Ping
```
C:>ping /?

사용법: ping [-t] [-a] [-n count] [-l size] [-f] [-i TTL] [-v TOS]
            [-r count] [-s count] [[-j host-list] | [-k host-list]]
            [-w timeout] [-R] [-S srcaddr] [-c compartment] [-p]
            [-4] [-6] target_name

옵션:
    -l size        송신 버퍼 크기입니다.
    -f             패킷에 조각화 안 함 플래그를 설정(IPv4에만 해당)합니다.
```
- 일반적인 상황에서의 Ping
  - 표준 이더넷 MTU 1500 바이트
  - ICMP 헤더: 8 바이트
  - IP 헤더: 20 바이트
  - 최대 MSS: 1472 바이트(1500-8-20)
```
C:\Users\netcanuck>ping 172.16.32.1 -f -l 1472

Pinging 172.16.32.1 with 1472 bytes of data:
Reply from 172.16.32.1: bytes=1472 time=3ms TTL=251
Reply from 172.16.32.1: bytes=1472 time=4ms TTL=251
Reply from 172.16.32.1: bytes=1472 time=4ms TTL=251
Reply from 172.16.32.1: bytes=1472 time=3ms TTL=251

C:\Users\netcanuck>ping 172.16.32.1 -f -l 1473

Pinging 172.16.32.1 with 1473 bytes of data:
Packet needs to be fragmented but DF set.
Packet needs to be fragmented but DF set.
Packet needs to be fragmented but DF set.
Packet needs to be fragmented but DF set.
```
- IPSec 터널에서의 Ping
  - 표준 이더넷 MTU 1500 바이트
  - ICMP 헤더: 8 바이트
  - IP 헤더: 20 바이트
  - IPSec 헤더: 88 바이트 (이것은 직접 계산하고 테스트해서 찾아야 함)
  - 최대 MSS: 1384 바이트(1500-8-20-88)
```
C:\Users\netcanuck>ping 172.16.68.1 -f -l 1472

Pinging 172.16.68.1 with 1472 bytes of data:
Packet needs to be fragmented but DF set.
Packet needs to be fragmented but DF set.
Packet needs to be fragmented but DF set.
Packet needs to be fragmented but DF set.

C:\Users\netcanuck>ping 172.16.68.1 -f -l 1384

Pinging 172.16.68.1 with 1472 bytes of data:
Reply from 172.16.68.1: bytes=1384 time=8ms TTL=251
Reply from 172.16.68.1: bytes=1384 time=9ms TTL=251
Reply from 172.16.68.1: bytes=1384 time=9ms TTL=251
Reply from 172.16.68.1: bytes=1384 time=8ms TTL=251
```

### Header 구조
- TCP Header
![TCP Header](./img/tcp_header.png "TCP Header")

- IP Header
![IP Header](./img/ip_header.png "IP Header")

- UDP Header
![UDP Header](./img/udp_header.png "UDP Header")

- ICMP Header
![ICMP Header](./img/icmp_header.png "ICMP Header")

- ARP Header
![ARP Header](./img/arp_header.jpg "ARP Header")
