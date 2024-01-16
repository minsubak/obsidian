## 도커 명령어 - inspect

- 개요
```txt
`inspect` 명령어는 도커 객체의 상세 정보를 JSON 형식으로 반환한다. 컨테이너, 이미지, 네트워크, 볼륨 등 다양한 도커 객체의 상세 정보를 확인할 수 있다.
```

- 명령어 기본 구성
```bash
$docker inspect -option <object>
```

- 명령어 구성 설명
```bash
-option  : 명령에 추가할 옵션
<object> : 상세정보를 출력할 객체
```

- option 태그
```bash
--format : 출력 형식을 지정
```

#도커