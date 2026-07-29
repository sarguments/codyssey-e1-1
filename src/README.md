# 웹 서버 소스

이 디렉터리에는 Dockerfile 기반 웹 서버 이미지에 포함할 파일을 둡니다.

- `Dockerfile`: `nginx:alpine`을 베이스 이미지로 사용한다.
- `ENV APP_ENV=dev`: 컨테이너 안의 기본 환경 변수 `APP_ENV`를 `dev`로 설정한다.
- `index.html`: 컨테이너에서 제공할 정적 웹 페이지다.
- `.dockerignore`: 이미지 빌드 컨텍스트에서 제외할 파일을 지정한다.

저장소 루트에서 아래 명령으로 빌드한다.

```bash
docker build -t e1-web:1.0 ./src
```
