## 도커 명령어 - push

- 개요
```txt
`push` 명령어는 로컬 시스템에 존재하는 도커 이미지를 도커 레지스트리로 업로드하는데 사용한다. 이미지를 레지스트리로 푸시하면, 해당 이미지를 원격에서 사용할 수 있게 된다.
```

- 명령어 기본 구성
```bash
$docker push -option <image:tag>
```

- 명령어 구성 설명
```bash
-option     : 명령에 추가할 옵션
<image:tag> : 업로드할 이미지 이름과 태그
```

- option 태그
```bash
--disable-content-trust : 이미지에 대한 content trust 검증을 비활성화
```

#도커