## 도커 명령어 - exec

- 개요
```txt
`exec` 명령어는 실행 중인 도커 컨테이너 내에서 명령을 실행한다. 이를 통해 컨테이너 내부에서 상호작용을 할 수 있다.
```

- 명령어 기본 구성
```bash
$docker exec -option <container> [command] [arg...]
```

- 명령어 구성 설명
```bash
-option     : 명령에 추가할 옵션
<container> : 명령을 실행할 컨테이너의 ID 또는 이름
[command]   : 컨테이너 내에서 실행할 명령어
[arg...]    : 명령에 전달할 추가 인자
```

- option 태그
```bash
-d, --detach : 컨테이너에서 명령을 백그라운드에서 실행
--env        : 환경 변수를 설정
--user       : 실행할 사용자를 지정
--workdir    : 실행할 명령을 위한 작업 디렉토리를 지정
-it          : 명령어 실행 시 터미널 상호작
```

#도커