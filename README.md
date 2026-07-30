# 개발 워크스테이션 미션

- 제출 저장소: [sarguments/codyssey-e1-1](https://github.com/sarguments/codyssey-e1-1)

## 프로젝트 개요

이 저장소는 터미널, Docker, Git/GitHub를 직접 사용해 팀원 누구나 같은 방식으로 실행, 배포, 디버깅할 수 있는 개발 워크스테이션을 만드는 미션의 기록이다.

- 터미널로 작업 디렉터리와 파일 권한을 다룬다.
- Docker를 설치·점검하고 이미지와 컨테이너를 운영한다.
- 웹 서버를 Dockerfile로 컨테이너화하고 포트 매핑, 바인드 마운트, Docker 볼륨을 확인한다.
- Git 설정과 GitHub 및 VS Code 연동을 기록한다.

## 실행 환경

- OS: macOS 15.7.4
- Shell: zsh
- Terminal: macOS 기본 Terminal 앱
- Docker: 28.5.2
- Git: 2.53.0

## 수행 항목 체크리스트

- [x] 터미널 조작 로그 기록
- [x] 파일·디렉터리 권한 변경
- [x] Docker 설치 및 기본 점검
- [x] Docker 기본 운영 명령 수행
- [x] `hello-world`와 Ubuntu 컨테이너 실행
- [x] Dockerfile 기반 커스텀 이미지 제작
- [x] 포트 매핑 및 접속 확인
- [x] 바인드 마운트 변경 반영
- [x] Docker 볼륨 영속성
- [x] Git 설정 및 GitHub·VS Code 연동
- [x] 트러블슈팅 2건 이상

## 수행 기록과 검증 방법

명령과 출력 전체는 연결된 로그 문서에 기록했다.

| 수행 항목 | 확인 방법 및 결과 | 기록·증거 |
|---|---|---|
| 터미널 조작 | `pwd`, `ls -la`, `cd`, `mkdir`, `printf`, `cp`, `mv`, `rm`, `cat`, `touch`로 경로 이동과 파일 상태 변화를 확인했다. | [터미널 조작 로그](docs/terminal-log.md#터미널-조작) |
| 파일·디렉터리 권한 | `ls -l`, `ls -ld`, `chmod`로 파일과 디렉터리의 권한 변경 전후를 비교했다. | [권한 실습 로그](docs/terminal-log.md#권한-실습) |
| Docker 설치·점검 | `docker --version`으로 버전을, `docker info`로 Docker 데몬 동작을 확인했다. | [설치 및 점검 로그](docs/docker-log.md#docker-설치-및-기본-점검) |
| Docker 기본 운영 | 이미지 다운로드·목록, 컨테이너 실행·중지·목록, 로그와 리소스 사용량을 확인했다. | [기본 운영 로그](docs/docker-log.md#docker-기본-운영-명령) |
| 컨테이너 실행 | `hello-world`와 Ubuntu 컨테이너를 실행하고, `attach`와 `exec` 사용 뒤의 컨테이너 상태 차이를 확인했다. | [컨테이너 실행 로그](docs/docker-log.md#컨테이너-실행-실습) |
| 커스텀 이미지 | `nginx:alpine`을 기반으로 `e1-web:1.0` 이미지를 빌드하고 컨테이너 실행을 확인했다. | [Dockerfile](src/Dockerfile) · [실행 로그](docs/docker-log.md#dockerfile-기반-커스텀-이미지) |
| 포트 매핑 | `-p 8080:80`으로 실행한 뒤 `curl` 응답과 브라우저 접속을 확인했다. | [실행 로그](docs/docker-log.md#포트-매핑-및-접속) · [접속 화면](evidence/docker-port-for.png) |
| 바인드 마운트 | 호스트의 HTML 파일을 변경한 뒤 컨테이너의 응답이 바뀌는지 확인했다. | [바인드 마운트 로그](docs/docker-log.md#바인드-마운트-반영) |
| Docker 볼륨 | 첫 컨테이너를 삭제한 뒤 같은 볼륨을 연결한 새 컨테이너에서 기존 데이터를 확인했다. | [볼륨 영속성 로그](docs/docker-log.md#docker-볼륨-영속성) |
| Git·GitHub·VS Code | Git 사용자 정보와 기본 브랜치, 원격 저장소를 확인하고 VS Code 연동 화면을 기록했다. | [Git 설정 로그](docs/git-log.md#git-설정-및-github-연동) · [연동 화면](evidence/vscode-github.png) |

## 커스텀 이미지 구성

- 베이스 이미지: `nginx:alpine`
- 선택 이유: Nginx로 정적 페이지를 제공할 수 있고 Alpine 기반이라 이미지가 가볍다.
- `COPY`: 기본 페이지를 [직접 작성한 페이지](src/index.html)로 교체한다.
- `ENV`: 기본 환경 변수 `APP_ENV=dev`를 설정한다.
- `LABEL`: 이미지 제목을 `e1-web`으로 기록한다.

## 트러블슈팅

- [같은 이름의 컨테이너를 다시 생성할 때 발생한 충돌](docs/troubleshooting.md#사례-1)
- [바인드 마운트와 Docker 볼륨에서 사용하는 `-v` 옵션 구분](docs/troubleshooting.md#사례-2)

## 저장소 구조

```text
codyssey-e1-1/
├── README.md
├── src/                    # 웹 서버 소스코드 작성 위치
│   ├── Dockerfile
│   ├── .dockerignore
│   └── index.html
├── practice/               # 터미널·권한 실습 작업 공간
│   ├── bind-mount/
│   ├── terminal/
│   └── permissions/
├── docs/
│   ├── terminal-log.md     # 터미널·권한 실습 기록
│   ├── docker-log.md       # Docker 실습 기록
│   ├── git-log.md          # Git·GitHub 연동 및 보안 점검
│   └── troubleshooting.md  # 문제 해결 기록
└── evidence/               # 캡처 파일 위치
```
