## 도커 명령어 - logs

- 개요
```txt
`logs` 명령어는 컨테이너의 로그를 확인할 때 사용한다. 실행 중인 도커 컨테이너에서 출력되는 로그 메세지를 표시하고, 컨테이너의 동작을 모니터링하거나 디버깅하는 데 도움이 된다.
```

- 명령어 기본 구성
```bash
$docker logs -option <container>
```

- 명령어 구성 설명
```bash
-option     : 명령에 추가할 옵션
<container> : 기록을 확인할 컨테이너의 ID 또는 이름
```

- option 태그
```bash
-f, --follow : 로그를 실시간으로 출력(실시간 갱신)
--since      : 특정 시간으로부터의 로그를 출력(ex: `--since 2022-01-01T00:00:00`)
--until      : 특정 시간까지의 로그를 표시
--tail       : 마지막에 기록된 로그에서 특정 라인 수를 출력
```

#도커