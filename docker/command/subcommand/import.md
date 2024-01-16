## 도커 명령어 - import

- 개요
```txt
`import` 명령어는 내보낸 tar 아카이브를 다시 들여와 이미지로 만든다.
```

- 명령어 기본 구성
```bash
$docker import -option <file.tar> [image:tag]
```

- 명령어 구성 설명
```bash
-option     : 명령에 추가할 옵션
<file.tar>  : 들여올 tar 아카이브 파일 이름
[image:tag] : 새로 만들 이미지의 이름과 태그
```

- option 태그
```bash
-c, --change          : 이미지에 적용할 Dockerfile 명령을 지정
-m, --meesage         : 생성된 이미지에 대한 설명을 추가
--platform            : 생성된 이미지의 플랫폼을 지정
--change-annotations  : 이미지에 변경 사항을 추가할 때 사용하는 어노테이션을 설정
--meesage-annotations : 이미지에 대한 설명을 추가할 때 사용하는 어노테이션을 설정
```

#도커