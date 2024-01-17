## 도커 명령어 - plugin

- 개요
```txt
`plugin` 명령어는 도커에서 플러그인을 관리할 때 사용된다. 도커 플러그인은 다양한 기능을 확장하고 도커 환경을 사용자 정의하는데 도움을 주는 확장 모듈이다. 
```

- 명령어 기본 구성
```bash
$docker plugin <flag> <plugin>
```

- 명령어 구성 설명
```bash
<flag>   : 플러그인 명령에서 수행할 작업 플래그
<plugin> : 작업할 플러그인 ID 또는 이름
```

- flag 태그
```bash
ls(list)       : 현재 시스템에 설치된 도커 플러그인 목록 출력
inspect        : 특정 플러그인에 대한 자세한 정보를 출력
install        : 도커 플러글인을 설치
enable/disable : 특정 플러그인을 활성화/비활성화
rm(remove)     : 특정 플러그인을 제거
upgrade        : 특정 플러그인을 업그레이드
create         : 사용자 정의 도커 플러그인을 생성
```

#도커