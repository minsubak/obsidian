## 도커 명령어 - port

- 개요
```txt
`port` 명령어는 실행 중인 도커 컨테이너의 특정 포트 매핑 정보를 확인할 때 사용한다.
컨테이너 내에서 개방된 포트와 호스트 머신에서 사용 가능한 포트 간의 매핑 정보를 출력한다.

! 프로토콜을 지정하지 않을 경우 TCP 프로토콜이 default로 사용된다 !
```

- 명령어 기본 구성
```bash
$docker port <container> [port/protocol]
```

- 명령어 구성 설명
```bash
<container>     : 포트 매핑 정보를 확인할 컨테이너의 ID 또는 이름
[port/protocol] : 특정 포트/프로토콜에 대한 매핑 정보 확인
```

- option 태그
```bash

```

#도커