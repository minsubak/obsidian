## 도커 명령어 - network

- 개요
```txt
`network` 명령어는 도커에서 네트워크 관리와 관련된 작업을 수행할 때 사용한다. 도커 컨테이너 간 통신이나 컨테이너와 호스트 간 통신을 제어하기 위해 네트워크를 생성하고 구성한다.
```

- 명령어 기본 구성
```bash
$docker network <subcommand> <network>
```

- 명령어 구성 설명
```bash
<subcommand> : 네트워크 명령에서 수행할 작업 플래그
<network>    : 작업할 네트워크 이름
```

- subcommand 태그
```bash
create     : 새 도커 네트워크를 생성
ls(list)   : 시스템에 존재하는 도커 네트워크 목록 출력
inspect    : 특정 네트워크에 대한 상세 정보 출력
connect    : 컨테이너를 특정 네트워크에 연결
disconnect : 컨테이너를 특정 네크워크에서 분리
rm         : 도커 네트워크를 삭제
prune      : 사용되진 않는 네트워크를 정리
```

#도커