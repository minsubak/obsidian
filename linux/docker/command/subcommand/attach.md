## 도커 명령어 - attach

- 개요
```txt
`attach` 명령어는 실행 상태인 도커 컨테이너에 접속해 해당 컨테이너의 표준입출력(stdin, stdout)과 에러 스트림(stderr)을 현재 터미널에 연결할 때 사용한다. 입력 시 실행 중인 컨테이너의 콘솔에 실시간으로 입력이 가능하고 출력을 볼 수 있다.
```

- 명령어 기본 구성
```bash
$docker attach <container>
```

- 명령어 구성 설명
```bash
<container> : 컨테이너 ID 또는 이름
```

#도커 