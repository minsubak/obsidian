## 도커 명령어 - kill

- 개요
```txt
`kill` 명령어는 실행 중인 도커 컨테이너를 강제로 중지시키기 위해 사용한다. 컨테이너에 SIGKILL 시그널을 전송하여 컨테이너를 즉시 중지시킨다. 일반적으론 `stop` 명령어를 사용해 중지하는 것을 권장한다.
```

- 명령어 기본 구성
```bash
$docker kill -option <container>
```

- 명령어 구성 설명
```bash
-option     : 명령에 추가할 옵션
<container> : 중지시킬 컨테이너의 ID 또는 이름
```

- option 태그
```bash
-s, --signal : 전송할 시그널을 지정 (default : SIGKILL)
```

#도커