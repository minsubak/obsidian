#위키

subnet은 하나의 네트워크가 분할되어 나눠진 작은 네트워크를 뜻합니다. 이 네트워크를 분할하는 것을 subneting이라 하고, subneting은 subnet mask를 통해 수행합니다. subnet은 각 클래스로 나눠진 네트워크를 운영 중인 서비스의 규모에 맞추어 분할해 사용하여 낭비되는 IP 주소 자원을 최소화합니다. 또 네트워크의 규모를 줄여 broadcasting에 인한 부하를 줄이는 것 또한 가능합니다.

subnet은 gateway를 통해 서로 통신할 수 있습니다(ARP, routing).


# 참고자료
| 클래스 | IP 앞자리 | 최상위 비트 | 범위 | 호스트 | 네트워크  | 블록 |
| :--: | :--: | :--: | :--: | :--: | :--: | :--: |
| A | 0 ~ 126 | 0 | 0.0.0.0 ~ 127.0.0.0 | 16777216 | 128 | /8 |
| B | 128 ~ 191 | 1 | 128.0.0.0 ~ 192.225.0.0 | 65536 | 16384 | /16 |
| C | 192 ~ 223 | 11 | 192.0.0.0 ~ 223.255.255.0 | 256 | 2097152 | /24 |
| D | 224 ~ 239 | 111 | 224.0.0.0 ~ 239.255.255.255 | N/A | N/A | N/A |
| E | 240 ~ 255 | 1111 | 240.0.0.0 ~ 247.255.255.255 | N/A | N/A | N/A |
[[RFC 1918]]

