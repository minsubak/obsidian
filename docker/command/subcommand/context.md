## 도커 명령어 - context

- 개요
```txt
`context` 명령어는 도커 CLI에서 여러 도커 호스트를 관리하고 작업할 때 사용한다. 여러 호스트를 쉽게 전환하고 설정할 수 있으며 명령을 실행할 수 있다.
```

- 명령어 기본 구성
```bash
$docker context <subcommand> <context> -option "host=<host>"
```

- 명령어 구성 설명
```bash
<subcommand> : 컨텍스트 명령에서 수행할 작업 플래그
<context>    : 컨텍스트의 이름
-option      : 명령문에 추가할 옵션
<host>       : 연결할 도커 호스트의 주소 또는 연결 정보
```

- option 태그
```bash
-d, --description            : 도커 컨텍스트에 대한 설명을 추가
--default-stack-orchestrator : 도커 컨텍스트의 기본 스택 오케스트레이터를 설정
--docker                     : 도커 호스트 연결을 위한 옵션, 호스트 유형 및 연결 정보를
                              지정, SSH, TCP 연결 가능
--kubernetes                 : 쿠버네티즈 클러스터와 연결할 때 사용, 클러스터의 URL, 
                              인증 토큰을 지정
--swarm                      : 도커 스웜 클러스터와 연결할 때 사용, 스웜의 URL, CA인증서,                                  토큰 등을 설정
--help                       : 도움말 출력
```

#도커