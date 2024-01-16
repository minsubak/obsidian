## 도커 명령어 - cp

- 개요
```txt
`cp` 명령어는 도커 컨테이너에서 호스트 또는 호스트에서 컨테이너로 파일을 복사할 때 사용한다. 
시스템과 컨테이너 간 파일이나 디렉토리를 복사할 수 있다.
```

- 명령어 기본 구성
```bash
$docker cp <container>:<container_address> <host_address>
$docker cp <host_address> <container>:<container_address>

# 컨테이너 내의 /app 디렉토리에서 호스트의 현재 작업 디렉토리로 파일 복사
# 호스트의 현재 작업 디렉토리에서 컨테이너 내의 /app 디렉토리로 파일 복사
```

- 명령어 구성 설명
```bash
<container>         : 복사 작업을 수행할 컨테이너의 ID 또는 이름
<container_address> : 컨테이너의 주소
<host_address>      : 호스트의 주소
```

#도커