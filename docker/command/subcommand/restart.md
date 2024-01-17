## 도커 명령어 - restart

- 개요
```txt
`restart` 명령어는 도커 컨테이너를 재시작할 때 사용한다. 컨테이너를 재시작하면 모든 프로세스가 중지되고 다시 시작된다.
```

- 명령어 기본 구성
```bash
$docker restart -option <container>
```

- 명령어 구성 설명
```bash
-option     : 명령에 추가할 옵션
<container> : 재시작할 컨테이너의 ID 또는 이름
```

- option 태그
```bash
-t, --time : 컨테이너가 정지한 후 대기할 시간을 지정 (default : 10s)
```

#도커