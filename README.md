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

### 터미널 조작

- 실행 위치: 저장소 루트
- 확인 명령: `pwd`, `ls -la`, `cd practice/terminal`, `mkdir`, `printf`, `cp`, `mv`, `rm`, `cat`, `touch`
- 확인 기준: `pwd`로 현재 위치와 이동 뒤 위치를, `ls -la`로 숨김 파일을 포함한 목록과 각 파일 상태 변화를, 파일 내용 확인 명령으로 생성·복사·이름 변경·삭제·빈 파일 생성을 확인한다.
- 설명: 절대 경로는 `/`부터 시작해 현재 위치와 관계없이 같은 대상을 가리킨다. 상대 경로는 현재 작업 디렉터리를 기준으로 해석된다.
- 명령 설명: `printf`는 문자열을 출력하고, `>`는 그 출력을 파일에 덮어쓴다. `\n`은 줄바꿈이며, `>>`는 기존 파일 끝에 내용을 추가한다.
- 예시: `practice/terminal`에서 `workspace/original.txt`는 상대 경로다. 두 번째 `pwd` 출력 뒤에 `/workspace/original.txt`를 붙인 값은 같은 파일의 절대 경로다.
- 기록 위치: [터미널 조작 로그](docs/terminal-log.md#터미널-조작)

### 파일·디렉터리 권한

- 실행 위치: `practice/permissions`
- 확인 명령: `ls -l`, `ls -ld`, `chmod`
- 확인 기준: 파일 1개와 디렉터리 1개에서 `chmod` 전후의 권한 표기가 달라졌는지 확인한다.
- 설명:
  - `ls -l` 출력의 첫 글자는 항목 종류다. `-`는 일반 파일, `d`는 디렉터리이며, 뒤의 아홉 글자가 권한이다.
  - 세 자리 숫자는 왼쪽부터 소유자·그룹·기타 사용자의 권한이다.
  - `r=4`(2진수 `100`), `w=2`(2진수 `010`), `x=1`(2진수 `001`)이며, 필요한 값을 더해 한 자리를 만든다.
  - `7=4+2+1`은 `rwx`, `6=4+2`는 `rw-`, `5=4+1`은 `r-x`, `4`는 `r--`이다.
  - `755`는 소유자 `rwx`, 그룹 `r-x`, 기타 사용자 `r-x`이고, `644`는 소유자 `rw-`, 그룹 `r--`, 기타 사용자 `r--`이다.
- 기록 위치: [권한 실습 로그](docs/terminal-log.md#권한-실습)

### Docker 설치 및 기본 점검

- 확인 명령: `docker --version`, `docker info`
- 확인 기준: `docker --version`이 버전을 출력하고 `docker info`가 서버 정보를 출력하는지 확인한다.
- 기록 위치: [Docker 설치 및 점검 로그](docs/docker-log.md#docker-설치-및-기본-점검)

### Docker 기본 운영 명령

- 확인 명령: 이미지 다운로드·목록, 컨테이너 실행·중지·목록, `docker logs`, `docker stats`
- 확인 기준: 이미지 목록, 실행·중지된 컨테이너 목록, 컨테이너 로그, 리소스 사용량이 각각 출력되는지 확인한다.
- 설명: `nginx:alpine`에서 `nginx`는 이미지 이름, `alpine`은 Alpine Linux 기반 변형을 고르는 태그다. `docker stats --no-stream`은 리소스 사용량을 한 번만 출력하고 종료한다.
- 기록 위치: [Docker 기본 운영 로그](docs/docker-log.md#docker-기본-운영-명령)

### 컨테이너 실행 실습

- 확인 명령: `docker run hello-world`, Ubuntu 컨테이너 실행, 내부 `ls`·`echo`, `attach` 또는 `exec`
- 확인 기준: `hello-world` 성공 메시지, Ubuntu 내부의 `ls`·`echo` 출력, `attach`·`exec` 사용 뒤 컨테이너 상태를 확인한다.
- 설명: `attach`는 컨테이너의 주 프로세스 입출력에 연결하고, `exec`는 실행 중인 컨테이너 안에서 별도 프로세스를 실행한다. 실제 셸 또는 프로세스를 종료한 뒤의 상태 차이는 실행 결과로 기록한다.
- 명령 설명: `docker run -dit ... ubuntu bash -i`에서 `-dit`는 Docker의 백그라운드·표준 입력·가상 터미널 옵션이고, 마지막 `bash -i`는 Bash를 대화형으로 실행하는 별도 옵션이다.
- 기록 위치: [컨테이너 실행 로그](docs/docker-log.md#컨테이너-실행-실습)

### Dockerfile 기반 커스텀 이미지

- 선택한 기존 베이스 이미지: `nginx:alpine`
- 선택 이유: Nginx가 포함돼 정적 웹 페이지를 바로 제공할 수 있고, Alpine 기반이라 이미지가 가볍다.
- 커스텀 포인트:
  - `COPY index.html /usr/share/nginx/html/index.html`: 기본 페이지 대신 내 페이지를 보여 준다.
  - `ENV APP_ENV=dev`: 컨테이너 안의 기본 환경 변수 `APP_ENV`를 `dev`로 설정한다.
  - `LABEL org.opencontainers.image.title="e1-web"`: 이미지 제목을 나타내는 메타데이터를 설정한다.
- 빌드 명령: `docker build -t e1-web:1.0 ./src`
- 실행 명령: `docker run -d --name e1-web e1-web:1.0`
- 확인 기준: 선택한 베이스 이미지와 커스텀 내용의 목적을 적고, 빌드 성공 및 컨테이너 실행을 확인한다.
- 설명: 이미지는 실행 환경을 정의한 템플릿이고, 컨테이너는 그 이미지를 실제로 실행한 인스턴스다. Dockerfile의 `FROM`은 베이스 이미지를 선택하고, 이후 명령으로 필요한 커스텀을 적용한다.
- Dockerfile 명령: `COPY`는 파일을 이미지에 넣고, `RUN`은 이미지를 만들 때 명령을 실행하며, `ENV`는 컨테이너의 기본 환경 변수를 설정한다. 이 Dockerfile은 `COPY`와 `ENV`를 사용하고 `RUN`은 사용하지 않는다.
- 관련 파일: [Dockerfile](src/Dockerfile), [웹 서버 페이지](src/index.html), [.dockerignore](src/.dockerignore)
- 기록 위치: [커스텀 이미지 로그](docs/docker-log.md#dockerfile-기반-커스텀-이미지)

### 포트 매핑 및 접속

- 확인 명령: `docker run -d --name e1-web-port -p 8080:80 e1-web:1.0`, `docker ps`, `curl`
- 확인 기준: `docker ps`에 호스트 포트와 컨테이너 포트 연결이 표시되고, `curl` 응답과 브라우저 접속 화면에서 모두 응답을 확인한다.
- 설명: `-p 8080:80`처럼 왼쪽은 호스트 포트, 오른쪽은 컨테이너 포트다. 브라우저의 `localhost:8080` 요청은 Docker를 거쳐 컨테이너의 `80` 포트로 전달된다.
- 명령 설명: `curl -i`의 `-i`는 HTTP 상태·헤더와 본문을 함께 출력하는 curl 옵션이다. `HTTP/1.1 200 OK`를 통해 서버 응답 성공을 확인할 수 있다.
- 기록 위치: [포트 매핑 로그](docs/docker-log.md#포트-매핑-및-접속)

### 바인드 마운트

- 확인 명령: 바인드 마운트를 포함한 `docker run`, 호스트 파일 변경, 응답 확인
- 확인 기준: 호스트 파일을 바꾼 뒤 컨테이너의 응답 또는 파일 내용도 바뀌는지 확인한다.
- 설명: 바인드 마운트는 호스트의 특정 경로를 컨테이너 경로에 직접 연결한다. 따라서 호스트 파일 변경을 컨테이너에서 바로 확인해야 하는 개발 환경에 적합하다.
- 기록 위치: [바인드 마운트 로그](docs/docker-log.md#바인드-마운트-반영)

### Docker 볼륨 영속성

- 확인 명령: 볼륨 생성, 컨테이너 연결, 데이터 작성, 컨테이너 삭제, 새 컨테이너 재연결
- 확인 기준: 첫 컨테이너에서 만든 데이터를 삭제 후 새 컨테이너에서도 읽을 수 있는지 확인한다.
- 설명: Docker 볼륨은 컨테이너 파일 시스템과 분리되어 관리되므로, 컨테이너를 삭제해도 볼륨 자체를 삭제하지 않으면 데이터가 유지된다.
- 바인드 마운트와 차이: 바인드 마운트는 호스트 경로를 직접 사용하고, Docker 볼륨은 Docker가 관리하는 저장 공간을 사용한다.
- 기록 위치: [Docker 볼륨 로그](docs/docker-log.md#docker-볼륨-영속성)

### Git 설정 및 GitHub·VS Code 연동

- 확인 명령: Git 사용자 정보·기본 브랜치 설정, `git config --list`, 원격 저장소 확인
- 확인 기준: `git config --list`에 사용자 정보와 기본 브랜치 설정이 표시되고, 원격 저장소, GitHub 연동, VS Code 연동 증거를 확인한다.
- 설명: Git은 로컬에서 변경 이력을 관리하는 버전 관리 도구이고, GitHub는 Git 저장소를 원격으로 공유하고 협업할 수 있게 하는 플랫폼이다.
- 기록 위치: [Git 및 GitHub 로그](docs/git-log.md#git-설정-및-github-연동)

## 트러블슈팅

- 기록 위치: [트러블슈팅 기록](docs/troubleshooting.md)

## 저장소 구조

```text
codyssey-e1-1/
├── README.md
├── src/                    # 웹 서버 소스코드 작성 위치
│   ├── Dockerfile
│   ├── .dockerignore
│   ├── index.html
│   └── README.md
├── practice/               # 터미널·권한 실습 작업 공간
│   ├── bind-mount/
│   ├── terminal/
│   └── permissions/
├── docs/
│   ├── terminal-log.md     # 터미널·권한 실습 기록
│   ├── docker-log.md       # Docker 실습 기록
│   ├── git-log.md          # Git·GitHub 연동 및 보안 점검
│   └── troubleshooting.md  # 문제 해결 기록
└── evidence/               # 캡쳐 파일 위치
```
