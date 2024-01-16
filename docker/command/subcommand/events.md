## 도커 명령어 - events

- 개요
```txt
`events` 명령어는 도커에서 발생하는 이벤트를 실시간으로 모니터링할 수 있게 한다. 도커 데몬에서 일어나는 다양한 사건들을 실시간으로 확인할 수 있다.
```

- 명령어 기본 구성
```bash
$docker events -option
```

- 명령어 구성 설명
```bash
-option : 명령에 추가할 옵션
```

- option 태그
```bash
-f, -filter : 특정 이벤트를 필터링하는데 사용
-format     : 출력 형식 지정에 사용
```

- 이벤트 필터링 태그
```bash
# 이벤트 목록에 표시되는 대표적인 이벤트 종류
start, stop, kill, create, destroy
```

#도커