## 도커 명령어 - commit

- 개요
```txt
`commit` 명령어는 도커 컨테이너의 상태를 새 도커 이미지로 저장할 때 사용한다. 현재 실행 중인 컨테이너에서 작업한 내용을 이미지로 저장해 추후 재사용하거나 공유하기 용이하다.
```

- 명령어 기본 구성
```bash
$docker commit -option <container> [<address>:<tag>]
```

- 명령어 구성 설명
```bash
-option     : 명령어에 추가할 옵션
<container> : 이미지로 저장할 컨테이너의 ID 또는 이름
<address>   : 새 이미지를 저장할 도커 레지스트리 저장소
<tag>       : 새 이미지를 저장할 때 지정할 태그
```

- option 태그
```bash
-a, --author : 이미지를 생성한 사용자의 정보를 설정
-c, --change : 이미지 생성 시, 적용할 Dockerfile 명령을 지정
--message    : 이미지에 대한 설명 또는 주석을 추가
```

#도커