# 개발 워크스테이션 미션

## 프로젝트 개요

- 개발 워크 스테이션 (팀원 누구나 같은 방식으로 실행, 배포, 디버깅 할 수 있는 환경) 만들기
- 설명 가능
	- 절대 경로와 상대 경로의 차이를 예시를 들어 설명
	- 파일 권한의 의미(r/w/x)와 755, 644 같은 표기가 어떤 규칙으로 해석되는지 설명
	- 기존 Dockerfile을 기반으로 “커스텀 이미지” 만들기
	- 포트 매핑이 필요한 이유
	- Docker 볼륨(영속 데이터)
	- Git과 GitHub의 역할 차이(로컬 버전관리 vs 원격 협업 플랫폼)

## 수행하는 것들
- 터미널로 작업 디렉토리와 권한 정리
- 도커를 설치 및 점검하고 컨테이너를 실행/관리
- 웹서버를 도커파일로 컨테이너화하고, 포트매핑으로 접속 확인하며, 바인드 마운트로 "변경 반영"/볼륨으로 "데이터 영속성"을 검증

## 실행 환경

| 항목 | 값 | 확인 명령 |
|---|---|---|
| OS | macOS 15.7.4 | `sw_vers` |
| Shell | zsh | `echo $SHELL` |
| Git | 2.53.0 | `git --version` |
| Docker | 28.5.2 | `docker --version` |

## 수행 체크리스트

- [ ] 터미널 조작 로그 기록
- [ ] 권한 실습 기록
- [ ] Docker 설치 및 기본 점검
- [ ] Docker 기본 운영 명령 수행
- [ ] 컨테이너 실행 실습
- [ ] 기존 Dockerfile 기반 커스텀 이미지 제작
- [ ] 포트 매핑 및 접속 증거
- [ ] 바인드 마운트 반영 검증
- [ ] Docker 볼륨 영속성 검증
- [ ] Git 설정 및 GitHub 연동
- [ ] 민감정보 노출 여부 최종 점검

## 검증 방법

| 검증 항목 | 기록 위치 | 필수 기록 내용 |
|---|---|---|
| 터미널 조작 로그 기록 | [`docs/terminal-log.md`](docs/terminal-log.md#터미널-조작-로그-기록) | 경로 확인, 파일·디렉터리 조작 명령과 출력 |
| 권한 실습 기록 | [`docs/terminal-log.md`](docs/terminal-log.md#권한-실습-기록) | 파일·디렉터리 권한의 변경 전후와 관찰 결과 |
| Docker 설치 및 기본 점검 | [`docs/docker-log.md`](docs/docker-log.md#docker-설치-및-기본-점검) | 버전, 엔진 상태 확인 명령과 출력 |
| Docker 기본 운영 명령 수행 | [`docs/docker-log.md`](docs/docker-log.md#docker-기본-운영-명령-수행) | 이미지, 컨테이너, 로그, 리소스 확인 결과 |
| 컨테이너 실행 실습 | [`docs/docker-log.md`](docs/docker-log.md#컨테이너-실행-실습) | `hello-world`, Ubuntu 실행 명령과 출력 |
| 기존 Dockerfile 기반 커스텀 이미지 제작 | [`docs/docker-log.md`](docs/docker-log.md#기존-dockerfile-기반-커스텀-이미지-제작) | 베이스 이미지, 커스텀 내용, 빌드 결과 |
| 포트 매핑 및 접속 증거 | [`docs/docker-log.md`](docs/docker-log.md#포트-매핑-및-접속-증거) | 실행 명령, 터미널 응답, 브라우저 증거 |
| 바인드 마운트 반영 검증 | [`docs/docker-log.md`](docs/docker-log.md#바인드-마운트-반영-검증) | 실행 명령과 변경 전후 비교 |
| Docker 볼륨 영속성 검증 | [`docs/docker-log.md`](docs/docker-log.md#docker-볼륨-영속성-검증) | 컨테이너 삭제 전후 데이터 확인 |
| Git 설정 및 GitHub 연동 | [`docs/git-log.md`](docs/git-log.md#git-설정-및-github-연동) | Git 설정, 원격 저장소, 푸시 확인 |
| 민감정보 노출 여부 최종 점검 | [`docs/git-log.md`](docs/git-log.md#민감정보-노출-여부-최종-점검) | 커밋 대상과 공개 금지 정보 확인 결과 |

## 트러블 슈팅
- 기록 위치: [`docs/troubleshooting.md`](docs/troubleshooting.md)

## 저장소 구조

```text
codyssey/
├── README.md
├── src/                    # 웹 서버 소스코드 작성 위치
│   └── README.md
├── docs/
│   ├── terminal-log.md     # 터미널·권한 실습 기록
│   ├── docker-log.md       # Docker 실습 기록
│   ├── git-log.md          # Git·GitHub 연동 및 최종 점검 기록
│   └── troubleshooting.md  # 문제 해결 기록
└── evidence/
    └── README.md           # 캡처 파일 관리 규칙
```

## 보안 원칙

- 토큰, 비밀번호, 개인키, 인증 코드는 커밋하지 않습니다.
- 사용자명·이메일·로컬 절대 경로는 공개 전 필요에 따라 마스킹합니다.
- `.env` 파일은 버전 관리에서 제외하고, 필요한 경우 값이 없는 `.env.example`만 공유합니다.
