## 도커 명령어 - load

- 개요
```txt
`load` 명령어는 이전에 `save`로 내보낸 tar 아카이브를 불러와 복원할 때 사용한다. 
```

- 명령어 기본 구성
```bash
$docker load -option <input_file.tar>
```

- 명령어 구성 설명
```bash
-option          : 명령에 추가할 옵션
<input_file.tar> : 불러들일 tar 아카이브
```

- option 태그
```bash
-i, --input : 입력 파일을 지정
```

#도커