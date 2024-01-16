## 도커 명령어 - export

- 개요
```txt
`export` 명령어는 파일 시스템의 컨테이너 내용을 tar 아카이브로 내보낼 때 사용한다. 내보낸 tar 아카이브는 로컬 파일 시스템에 저장되거나 다른 곳으로 전송해 다시 import하거나 공유할 수 있다.
```

- 명령어 기본 구성
```bash
$docker export -option <container> > <output_file.tar>
```

- 명령어 구성 설명
```bash
-option           : 명령에 추가할 옵션
<container>       : 작업을 수행할 컨테이너의 ID 또는 이름
<output_file.tar> : 내보낼 tar 아카이브 이름
```

- option 태그
```bash
-o, --output : 출력 파일을 지정
```

#도커