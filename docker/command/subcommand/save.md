## 도커 명령어 - save

- 개요
```txt
`save` 명령어는 도커 이미지를 tar 아카이브 파일로 저장할 때 사용한다. 아카이브 파일은 이미자와 해당 레이어를 저장하고, 후에 다른 도커 호스트로 복사하거나 이관할 때 사용한다.
```

- 명령어 기본 구성
```bash
$docker save -option <image.tar> <image:tag>
```

- 명령어 구성 설명
```bash
-option     : 명령에 추가할 옵션
<image.tar> : 저장할 tar 아카이브 파일 이름
<image:tag  : 저장할 이미지 파일 이름
```

- option 태그
```bash
-o, --output : 아카이브 파일의 경로 및 이름을 지정
```

#도커