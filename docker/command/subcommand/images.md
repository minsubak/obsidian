## 도커 명령어 - images

- 개요
```txt
'images' 명령어는 현재 시스템에 저장된 도커 이미지 목록을 표시하는 데 사용한다.
! `docker image ls` 로 대체되었으나 호환성을 위해 계속 지원 !
```

- 명령어 기본 구성
```bash
$docker images -option [repositorie:tag]
```

- 명령어 구성 설명
```bash
-option           : 명령에 추가할 옵션
[repositorie:tag] : 
```

- option 태그
```bash
-a, --all  : 모든 이미지를 출력
--digests  : 이미지에 대한 다이제스트를 출력
--format   : 출력 형식을 지정
--no-trunc : 결과를 축소하지 않고 전체 내용을 출력
```

#도커