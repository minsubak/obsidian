## 도커 명령어 - checkpoint

- 개요
```txt
'checkpoint' 명령어는 도커 컨테이너의 현재 상태를 체크포인트로 저장하거나 복원할 때 사용한다. 주로 컨테이너의 상태를 저장하거나 실험, 디버깅 또는 백업 목적으로 활용한다.
```

- 명령어 기본 구성
```bash
$docker checkpoint <subcommand> -option <container> <checkpoint>
```

- 명령어 구성 설명
```bash
<subcommand> : 체크포인트 명령어에서 수행할 작업 플래그
-option      : 명령어에 추가할 옵션
<container>  : 상태를 저장할 도커 컨테이너의 ID 또는 이름
<checkpoint> : 생성할 checkpoint 이름
```

- subcommand 태그
```bash
create : 체크포인트 생성
ls     : 체크포인트 목록 출력
rm     : 체크포인트 삭제
```

- option 태그
```bash
--leave-running : 컨테이너가 정지하지 않은 상태에서 체크포인트 생성 (default : false)
```

- 체크포인트 복원 ([[start]])
```bash
$docker start --checkpoint <checkpointName> <containerID || containerName>
```

#도커