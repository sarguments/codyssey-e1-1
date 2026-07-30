# 트러블슈팅 기록

## 사례 1

- 문제: 여러 번 테스트하던 도중 이미 실행 중인 `e1-web-bind`와 같은 이름으로 `docker run --name e1-web-bind nginx:alpine`를 실행하자 컨테이너 생성에 실패했다.
- 원인 가설: Docker 컨테이너 이름은 고유해야 하며, 기존 컨테이너가 같은 이름을 사용하고 있기 때문이다.
- 확인: `docker ps -a --filter name=e1-web-bind`에서 기존 컨테이너가 `Up` 상태임을 확인했고, 재실행 명령이 `Conflict. The container name "/e1-web-bind" is already in use` 오류를 출력했다.
- 해결/대안: 기존 컨테이너를 계속 사용할 때는 `docker start e1-web-bind`를 사용한다. 새 컨테이너가 필요하면 다른 `--name`을 지정하고, 기존 컨테이너가 더 이상 필요 없을 때만 `docker rm`으로 제거한다.

## 사례 2

- 문제: 바인드 마운트에도 `-v` 옵션을 사용해 Docker 볼륨이 생성되는 것인지 혼란스러웠다.
- 원인 가설: `-v`가 `--volume`의 짧은 이름이지만, 바인드 마운트와 이름 있는 Docker 볼륨 모두에 사용하는 범용 마운트 옵션이기 때문이다.
- 확인: `-v "$PWD/practice/bind-mount:/usr/share/nginx/html:ro"`의 왼쪽 값은 호스트 경로라 바인드 마운트이고, `-v e1-data:/data`의 왼쪽 값은 Docker가 관리하는 볼륨 이름이다.
- 해결/대안: 마운트 유형을 분명히 표시해야 할 때는 `--mount type=bind,...` 또는 `--mount type=volume,...` 문법을 사용한다.
