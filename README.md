# 개발 워크스테이션 미션

> [!NOTE]
> 이 저장소는 미션 수행을 위한 **시작점**입니다. 아직 Docker 이미지 빌드, 컨테이너 실행, 볼륨 검증, GitHub 연동은 수행하지 않았습니다.

## 프로젝트 개요

터미널, 파일 권한, Docker, Git/GitHub의 기본 흐름을 직접 실습하고 결과를 재현 가능한 문서로 정리합니다.

## 실행 환경

| 항목 | 값 | 확인 명령 |
|---|---|---|
| OS | macOS 15.7.4 | `sw_vers` |
| Shell | zsh | `echo $SHELL` |
| Git | 2.53.0 | `git --version` |
| Docker | 28.5.2 | `docker --version` |

## 수행 체크리스트

- [ ] 터미널 기본 조작 및 로그 기록
- [ ] 파일·디렉터리 권한 변경 실습
- [ ] Docker 설치 및 엔진 점검
- [ ] `hello-world`와 Ubuntu 컨테이너 실습
- [ ] Dockerfile 작성 및 커스텀 이미지 빌드
- [ ] 포트 매핑과 접속 검증
- [ ] 바인드 마운트 변경 반영 검증
- [ ] Docker 볼륨 영속성 검증
- [ ] Git 설정 확인 및 GitHub 저장소 연동
- [ ] 트러블슈팅 2건 이상 기록
- [ ] 민감정보 노출 여부 최종 점검

## 저장소 구조

```text
codyssey/
├── README.md
├── src/                    # 웹 서버 소스코드 작성 위치
│   └── README.md
├── docs/
│   ├── terminal-log.md     # 터미널·권한 실습 기록
│   ├── docker-log.md       # Docker 실습 기록
│   └── troubleshooting.md  # 문제 해결 기록
├── evidence/
    └── README.md           # 캡처 파일 관리 규칙
```

## 보안 원칙

- 토큰, 비밀번호, 개인키, 인증 코드는 커밋하지 않습니다.
- 사용자명·이메일·로컬 절대 경로는 공개 전 필요에 따라 마스킹합니다.
- `.env` 파일은 버전 관리에서 제외하고, 필요한 경우 값이 없는 `.env.example`만 공유합니다.
