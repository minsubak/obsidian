## 도커 명령어 - pull

- 개요
```txt
`pull` 명령어는 도커 허브나 다른 도커 레지스트리에서 이미지를 다운로드할 때 사용한다. 이미지를 로컬 시스템으로 받아오기 위해 쓰며, 다운로드한 이미지를 기반으로 컨테이너를 생성하거나 사용한다.
```

- 명령어 기본 구성
```bash
$docker pull -option <image:tag>
```

- 명령어 구성 설명
```bash
-option     : 명령에 사용할 옵션
<image:tag> : 받아올 이미지의 이름과 태그
```

- option 태그
```bash
-a, --all-tags          : 모든 태그를 다운로드
--disable-content-trust : 이미지에 대한 content trust 검증을 비활성화
--platform              : 다운로드할 이미지의 특정 플랫폼을 지정
```

#도커