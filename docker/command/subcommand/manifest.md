## 도커 명령어 - manifest

- 개요
```txt
`manifest` 명령어는 다중 아키텍쳐 이미지의 manifest 정보를 조작하고 확인할 때 사용한다. 다중 아키텍쳐 이미지는 동일한 이미지를 여러 아키텍쳐에 대해 빌드하여 제공하는 경우에 유용하다.
```

- 명령어 기본 구성
```bash
$docker manifest <flag> [command] [server/image:tag]
```

- 명령어 구성 설명
```bash
<flag>       : 매니페스트 명령에서 수행할 작업 플래그
[command]          : 
[server/image:tag] : 작업에 사용할 다중 아키텍쳐 이미지의 이름과 태그
```

- flag 태그
```bash
inspect    : 이미지 manifest에 대한 상세 정보를 출력
create     : 이미지 manifest를 생성
annotate   : manifest에 추가 정보를 추가
push       : manifest를 레지스트리로 푸시
rm, delete : manifest를 로컬에서 제거
```

#도커