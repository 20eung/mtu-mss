# MTU, MSS 이야기
### MTU(Maximum Transmission Unit)
- 네트워크에서 한 번에 보낼 수 있는 최대 데이터 크기(헤더 포함)
- 단위는 Outet(옥텟)이며 Byte와 같은 사이즈(1 Outet = 1 Byte)
- 일반적인 이더넷에서 수용할 수 있는 크기는 1,500바이트
- MTU는 2계층의 데이터값

### MSS(Maximum Segment Size)
- MSS는 4계층에서 가질 수 있는 최대 데이터 값
- 순수하게 MTU에서 각종 헤더를 제외한 값
- 일반적인 IP 표준 헤더 20바이트, TCP 표준 헤더 20바이트(추가되는 옵션 헤더 제외)
- 일반적인 이더넷인 경우, MSS 값은 1,460바이트(1500-20-20)

### Header 크기
-   TCP Header: 20 Bytes(옵션 헤더 제외)
-   UDP Header:  8 Bytes
-    IP Header: 20 Bytes
-  ICMP Header:  8 Bytes
- IPSec Header: 20 Bytes

#### IPSec Header
- ESP Overhead
  - Outer IP Header:       20 Bytes
  - Sequence Number:        4 Bytes
  - SPI:                    4 Bytes
  - Initialization Vector: 16 Bytes(AES)
  - ESP Padding:        [0-15]Bytes
  - Padding Length:         1 Bytes
  - Next Header:            1 Bytes
  - Authentication Data:   32 Bytes(SHA-512)
  - Total:             [78-93]Bytes

- Initialization Vector
  - AES: 16 Bytes
  - DES:  8 Bytes

- Authentication Data
  - MD5/SHA-1: 12 Bytes
  - SHA-256:   16 Bytes
  - SHA-384:   24 Bytes
  - SHA-512:   32 Bytes

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
