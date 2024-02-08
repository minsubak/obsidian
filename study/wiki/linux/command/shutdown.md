## 데비안 명령어 - shutdown

- 개요
```txt
shutdown 명령은 linux 시스템의 안전한 종료와 재부팅을 위해 사용한다.
관리자 권한이 요구되며, 시스템 종료/재부팅 신호를 H/W에 전달한다.
```

- 명령어 기본 구성
```bash
shutdown -option time "message"
```

- 명령어 구성 설명
```bash
-option   : shutdown 명령에 추가할 옵션
time      : shutdown 명령으로 종료 또는 재부팅에 소요될 시간
"message" : 사용자에게 출력할 메세지
```

- option 태그
```bash
-h : 시스템 종료
-r : 시스템 재부팅
-k : 시스템 종료/재부팅 없이 경고 메세지만 출력
-c : 예약된 종료/재부팅을 취소
```

- time 수식
```bash
now   : 즉시
+M    : M분 후
HH:MM : HH시 MM분
```

- 유사한 기능/연관된 명령어 ([[init]], [[systemctl]])
```bash
poweroff           // 종료
systemctl poweroff // 종료
systemctl reboot   // 재부팅
sudo init 0      // 종료
sudo init 6      // 재부팅
```

#데비안