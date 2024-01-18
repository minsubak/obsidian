## 도커 명령어 - config

- 개요
```txt
`config` 명령어는 도커 서비스의 설정의 관리에 사용한다. 도커 서비스는 여러 노드에서 실행되는 컨테이너의 그룹의 형태인데 `config` 명령어를 이용해 서비스에 대한 구성을 정의하고 업데이트하는 것이 가능하다.
```

- 명령어 기본 구성
```bash
$docker config <flag> <config> <file|directory|url>
```

- 명령어 구성 설명
```bash
<flag>         : 구성 명령에서 수행할 작업 플래그
<config>             : 새 구성의 이름
<file|directory|url> : 구성으로 사용될 파일, 디렉토리 또는 url
```

- flag 태그
```bash
create  : 새 구성을 생성
ls      : 현재 생성된 구성의 목록 출력
inspect : 구성에 대한 상세 정보를 출력
rm      : 특정 구성을 제거
```

- `docker service create` 명령에서 `--config` 옵션 : 서비스를 생성할 때 구성을 지정 ([[service]])
```bash
docker service create --config <config> <service>
```

- `docker service update` 명령에서 `--config-rm | --config-add` 옵션: 서비스의 업데이트 중 구성을 추가하거나 제거 ([[service]])
```bash
docker service update --config-rm(-add) <config> <service>
```

#도커