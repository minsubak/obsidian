#리눅스

| command | info | useage |  |
| :--: | :--: | ---- | ---- |
| ping | 네트워크 상태 및 호스트 간 연결 테스트 | `ping [address]` |  |
| traceroute |  |  |  |
| tcpdump |  |  |  |
| netstat | 네트워크 포트 상태 확인 | netstat `-option` | grep [port number] |
| nslookup |  |  |  |

tcp example
- **tcpdump -i eth0**: 인터페이스 eth0을 보여줌
- **tcpdump -i eth0 -c 10**: 10개만 dump
- **tcpdump -i eth0 tcp port 80**: TCP 80 포트로 통신하는 packet dump
- **tcpdump -i eth0 src 192.168.0.1:** source IP가 192.168.0.1인 packet dump
- **tcpdump -i eth0 dst 192.168.0.1**: dest IP가 192.168.0.1인 packet dump
- **tcpdump -i eth0 src 192.168.0.1 and tcp port 80**: dest IP가 192.168.0.1, TCP 80 port인 packet 보여줌
- **tcpdump -i eth0 dst 192.168.0.1**: 목적지IP가 192.168.0.1인 packet 보여줌
- **tcpdump host 192.168.0.1**: 특정 호스트 IP로 들어오거나 나가는 양방향 packet 모두 dump
- **tcpdump src 192.168.0.1**: 특정 호스트 중에서 source가 192.168.0.1인 것만 dump
- **tcpdump dst 192.168.0.1**: 특정 호스트 중에서 dest가 192.168.0.1인 것만 dump
- **tcpdump port 3389**: port 양방향으로 3389이면 dump
- **tcpdump src port 3389**: source port가 3389인 것만 dump
- **tcpdump dst port 3389**: dest port가 3389인 것만 dump
- t**cpdump udp and src port 53**: UDP이고 source port가 53이면 dump
- **tcpdump src 192.168.0.1 and not dst port 22**: source IP가 192.168.0.1이고 목적지 port가 22 가 아닌 packet dump
- **tcpdump -w tcpdump.log**: save result to bin type file
- **tcpdump -r tcpdump.log**: read the save file