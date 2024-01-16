## 도커 명령어 - diff

- 개요
```txt
`diff` 명령어는 `docker container`의 작업 플래그로 사용할 수 있으며, 특정 컨테이너에서 파일 시스템에 대한 변경 사항을 출력한다.
 `A`(Added)   : 추가된 파일/디렉토리
 `C`(Changed) : 수정된 파일
 `D`(Deleted) : 삭제된 파일
```

- 명령어 기본 구성
```bash
$docker container diff <container>
```

- 명령어 구성 설명
```bash
<container> : 명령을 수행할 컨테이너의 ID 또는 이름
```

#도커