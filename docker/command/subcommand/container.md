## 도커 명령어 - container

- 개요
```txt
`container` 명령어는 도커 컨테이너 관리를 위한 것으로, 컨테이너를 생성, 실행, 정지, 삭제하고 여러 컨테이너와 관련된 작업을 수행할 수 있다.
```

- 명령어 기본 구성
``` bash
$docker container <flag> -option <image> [command] [arg...]
```

- 명령어 구성 설명
```bash
<flag> : 컨테이너 명령에서 수행할 작업 플래그
-option      : 명령어에 추가할 옵션
<image>      : 사용할 도커 이미지의 ID 또는 이름
[command]    : 컨테이너 내에서 실행할 명령어
[arg...]     : 명령에 전달할 추가 인자
```

- flag 태그
```bash
create  : 새로운 컨테이너를 생성
diff    : 컨테이너에서 파일 시스템에 대한 변경 사항을 출력
start   : 정지된 컨테이너를 시작
stop    : 실행 중인 컨테이너를 정지
restart : 컨테이너를 재시작
ls      : 실행 중인 컨테이너 목록 출력
exec    : 실행 중인 컨테이너 내에서 명령을 실행
logs    : 컨테이너의 로그를 출력
inspect : 컨테이너에 대한 상세 정보 출력
rm      : 컨테이너를 삭제
prune   : 사용하지 않는 컨테이너를 삭제
```

- option 태그
```bash
-a, --attach      : 컨테이너를 실행하면서 표준입출력/오류를 연결 (create, start, restart)
-c, --cpu-shares  : CPU 사용량을 설정 (create)
-m, --memory      : 컨테이너에 할당할 메모리 사이즈를 지정 (create)
-i, --interactive : 컨테이너를 대화형으로 실행 (start, restart, exec)
-d, --detach      : 백그라운드에서 실행 (exec)
-t, --tty         : 가상 터미널을 할당 (exec)
--details         : 로그에 추가 정보를 출력 (logs)
-f, --follow      : 로그를 실시간으로 출력 (logs)
--format          : 출력 형식을 지정 (inspect)
-f, --force       : 실행 중인 컨테이너를 강제로 중지 및 삭제 (rm) \
                    확인 프롬프트 표시 없이 강제 실행 (prune)
-v, -volumes      : 컨테이너와 연결된 볼륨을 함께 삭제 (rm)
-a, --all         : 중지된 컨테이너를 포함해 모든 컨테이너를 표시 (ls)
--filter          : 지정된 조건에 따라 삭제할 컨테이너를 필터링 (ls, prune)
```

#도커