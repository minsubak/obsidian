## 도커 명령어 - history

- 개요
```txt
`history` 명령어는 도커 이미지의 생성 과정을 표시한다. 각 이미지 레이어에 대한 정보를 보여줌으로써 어떻게 구축되었는지 시간순으로 표시한다.
```

- 명령어 기본 구성
```bash
$docker history -option <image:tag>
```

- 명령어 구성 설명
```bash
-option     : 명령에 추가할 옵션
<image:tag> : 기록을 출력할 이미지의 이름과 태그
```

- option 태그
```bash
--format   : 출력 형식을 지정
--no-trunc : 결과를 축소하지 않고 전체 내용을 표시
```

- history 명령어 입력 시 출력 결과
```bash
IMAGE       CREATED       CREATED BY       SIZE       COMMENT
...         ...           ...              ...        ...
...         ...           ...              ...        ...
```

#도커