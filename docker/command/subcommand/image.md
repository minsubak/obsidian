## 도커 명령어 - image

- 개요
```txt
`image` 명령어는 도커 이미지와 관련된 작업 수행에 사용한다. 이미지를 검색하고, 생성하고, 관리하며 삭제할 수 있다.
```

- 명령어 기본 구성
```bash
$docker image <subcommand> -option <image:tag>
```

- 명령어 구성 설명
```bash
<subcommand> : 이미지 명령에서 수행할 작업 플래그
-option      : 명령에 추가할 옵션
<image:tag>  : 작업을 진행할 이미지의 이름과 태그
```

- subcommand 태그
```bash
ls      : 현재 시스템에 저장된 도커 이미지 목록을 표시
pull    : 도커 허브 또는 다른 레지스트리에서 이미지를 내려받기
push    : 이미지를 도커 허브 또는 다른 레지스트리로 올려보내기
build   : Dockerfile을 사용하여 새 이미지를 빌드
rm      : 이미지를 삭제
inspect : 이미지에 대한 자세한 정보를 표시
save    : 이미지를 tar 아카이브로 내보내기
load    : tar 아카이브에서 이미지를 로드
prune   : 사용되지 않는 이미지를 정리
```

- option 태그
```bash
-a, -all     : 모든 이미지를 표시 (ls)
-t, --tag    : 빌드된 이미지에 태그를 지정 (build)
-f, --fjile  : 사용할 Dockerfile의 경로를 지정 (build)
-f, --force  : 이미지를 강제로 삭제 (rm)
-o, --output : 출력 파일을 지정 (save)
-i, --input  : 입력 파일을 지정 (load)
```

#도커