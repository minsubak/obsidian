##데비안 명령어 리스트

###shutdown 시스템 종료 명령
```bash
/* 기본 명령어 구조 */
shutdown [option] time [message]

// option : shutdown 명령에 추가할 옵션
// time   : shutdown 명령으로 종료 또는 재부팅까지 소요할 시간
// message: 사용자에게 출력할 메세지

// option tag
// -h: 시스템 종료
// -r: 시스템 재부팅
// -k: 시스템 종료/재부팅 없이 경고 메세지만 출력
// -c: 예약된 종료/재부팅을 취소

// time modify
// now  : 즉시
// +M   : M분 후
// HH:MM: HH시 MM분
```