## 도커 명령어 - build

- 개요
```txt
`build` 명령어는 Dockerfile을 기반으로 도커 이미지의 생성할 때 사용한다.
Dockerfile은 도커 이미지를 빌드하기 위한 명령어와 설정들을 순차적으로 정의한 텍스트파일이다.
```

- 명령어 기본 구성
```bash
$docker build -option <image> <address>
```

- 명령어 구성 설명
```bash
-option   : 도커 빌드 명령에 추가할 옵션
<image>   : 빌드할 이미지 이름
<address> : Dockerfile이 위치한 디렉토리/Git 저장소 URL
```

- option 태그
```bash
-t, --tag   : 이미지에 태그를 지정
-f, --file  : 빌드할 Dockerfile의 경로를 지정(Default : .)
--build-arg : 빌드 중, Dockerfile 내 사용되는 인수를 설정
--no-cache  : 캐시를 사용하지 않고 새로 빌드
-q, --quiet : 빌드 과정의 출력을 최소화
```

#도커