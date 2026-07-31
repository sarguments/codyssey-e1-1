# Docker Compose 심화 과제 로그

> 이 문서는 직접 실행한 명령·출력과 그 결과 해석을 기록한다.

## 구성

- 설정 파일: [docker-compose.yml](../docker-compose.yml)
- 서비스: `web`, `probe`

| 서비스 | 구성 | 역할 |
| --- | --- | --- |
| `web` | `./src`의 Dockerfile로 빌드, `APP_ENV` 주입, `${HOST_PORT:-8080}:80` 포트 매핑 | Nginx로 커스텀 HTML 제공 |
| `probe` | `busybox:1.36`, `wget -qO- http://web/` 실행 | Compose 내부에서 `web` 서비스 응답 확인 |

## Docker Compose 구성 확인

### 실행 기록

```console
sarguments7021@cx2r7s4 codyssey-e1-1 % docker compose config
name: codyssey-e1-1
services:
  probe:
    command:
      - wget
      - -qO-
      - http://web/
    depends_on:
      web:
        condition: service_started
        required: true
    image: busybox:1.36
    networks:
      default: null
  web:
    build:
      context: /Users/sarguments7021/codyssey-e1-1/src
      dockerfile: Dockerfile
    networks:
      default: null
    ports:
      - mode: ingress
        target: 80
        published: "8080"
        protocol: tcp
networks:
  default:
    name: codyssey-e1-1_default
```

### 결과

- `web`과 `probe`가 모두 기본 네트워크에 연결되고, `probe`의 명령과 `web`의 `8080:80` 포트 매핑이 설정에 포함됨을 확인했다.

## Docker Compose 빌드 및 실행

### 실행 기록

```console
sarguments7021@cx2r7s4 codyssey-e1-1 % docker compose up -d --build
[+] Building 1.8s (9/9) FINISHED                                                                                                   
 => [internal] load local bake definitions                                                                                    0.0s
 => => reading from stdin 531B                                                                                                0.0s
 => [internal] load build definition from Dockerfile                                                                          0.0s
 => => transferring dockerfile: 170B                                                                                          0.0s
 => [internal] load metadata for docker.io/library/nginx:alpine                                                               1.5s
 => [internal] load .dockerignore                                                                                             0.0s
 => => transferring context: 60B                                                                                              0.0s
 => [internal] load build context                                                                                             0.0s
 => => transferring context: 32B                                                                                              0.0s
 => [1/2] FROM docker.io/library/nginx:alpine@sha256:4a73073bd557c65b759505da037898b61f1be6cbcc3c2c3aeac22d2a470c1752         0.0s
 => CACHED [2/2] COPY index.html /usr/share/nginx/html/index.html                                                             0.0s
 => exporting to image                                                                                                        0.0s
 => => exporting layers                                                                                                       0.0s
 => => writing image sha256:fbe10c8c44912b6c4725cde03ac7f9b2a4c8151dd5ceccacf6aea831e0a1b743                                  0.0s
 => => naming to docker.io/library/codyssey-e1-1-web                                                                          0.0s
 => resolving provenance for metadata file                                                                                    0.0s
[+] Running 3/3
 ✔ codyssey-e1-1-web                Built                                                                                     0.0s 
 ✔ Container codyssey-e1-1-web-1    Running                                                                                   0.0s 
 ✔ Container codyssey-e1-1-probe-1  Started  
```

### 결과

- `--build`는 서비스를 시작하기 전에 `web` 이미지 빌드를 수행하고, `-d`는 컨테이너를 백그라운드에서 실행한다.
- 출력의 `Built`, `Running`, `Started`로 `web` 이미지를 빌드하고 두 서비스를 시작한 것을 확인했다.

## 컨테이너 상태

### 실행 기록

```console
sarguments7021@cx2r7s4 codyssey-e1-1 % docker compose ps -a
NAME                    IMAGE               COMMAND                  SERVICE   CREATED       STATUS                      PORTS
codyssey-e1-1-probe-1   busybox:1.36        "wget -qO- http://we…"   probe     2 hours ago   Exited (0) 22 seconds ago   
codyssey-e1-1-web-1     codyssey-e1-1-web   "/docker-entrypoint.…"   web       3 hours ago   Up 3 hours                  0.0.0.0:8080->80/tcp, [::]:8080->80/tcp
```

### 결과

- `docker compose ps -a`는 실행 중인 컨테이너뿐 아니라 종료된 컨테이너까지 보여 준다.
- `web`은 `Up` 상태이며 호스트 `8080` 포트가 컨테이너 `80` 포트에 연결됐다.
- `probe`의 주 명령은 한 번의 HTTP 요청이므로, 요청이 성공한 뒤 `Exited (0)`으로 끝나는 것이 정상이다.

## 서비스 간 통신

### 실행 기록

```console
sarguments7021@cx2r7s4 codyssey-e1-1 % docker compose logs probe
probe-1  | <!doctype html>
probe-1  | <html lang="ko">
probe-1  |   <head>
probe-1  |     <meta charset="utf-8" />
probe-1  |     <meta name="viewport" content="width=device-width, initial-scale=1" />
probe-1  |     <title>E1-1 Docker Web Server</title>
probe-1  |   </head>
probe-1  |   <body>
probe-1  |     <h1>E1-1 Docker Web Server</h1>
probe-1  |     <p>Custom Nginx image is running.</p>
probe-1  |   </body>
probe-1  | </html><!doctype html>
probe-1  | <html lang="ko">
probe-1  |   <head>
probe-1  |     <meta charset="utf-8" />
probe-1  |     <meta name="viewport" content="width=device-width, initial-scale=1" />
probe-1  |     <title>E1-1 Docker Web Server</title>
probe-1  |   </head>
probe-1  |   <body>
probe-1  |     <h1>E1-1 Docker Web Server</h1>
probe-1  |     <p>Custom Nginx image is running.</p>
probe-1  |   </body>
```

### 결과

- 출력된 커스텀 HTML은 `probe`가 호스트 포트 `8080`을 거치지 않고 서비스 이름 `web`으로 Nginx에 요청해 응답을 받았다는 근거다.
- `probe`의 `wget -qO- http://web/`에서 `-q`는 BusyBox `wget`의 진행·상태 메시지를 숨기고, `-O -`는 받은 문서를 파일이 아닌 표준 출력으로 보낸다.

## 호스트 접속

### 실행 기록

```console
sarguments7021@cx2r7s4 codyssey-e1-1 % curl -i http://localhost:8080/
HTTP/1.1 200 OK
Server: nginx/1.31.3
Date: Fri, 31 Jul 2026 04:54:26 GMT
Content-Type: text/html
Content-Length: 302
Last-Modified: Fri, 31 Jul 2026 01:43:16 GMT
Connection: keep-alive
ETag: "6a6bfdb4-12e"
Accept-Ranges: bytes

<!doctype html>
<html lang="ko">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>E1-1 Docker Web Server</title>
  </head>
  <body>
    <h1>E1-1 Docker Web Server</h1>
    <p>Custom Nginx image is running.</p>
  </body>
</html>%
```

### 결과

- `curl -i`의 `-i`는 HTTP 헤더와 본문을 함께 출력한다.
- `HTTP/1.1 200 OK`와 커스텀 HTML은 호스트 `localhost:8080` 요청이 `web` 컨테이너의 Nginx까지 전달됐다는 근거다.
- 서비스 간 통신의 `http://web/`과 달리, 이 요청은 `8080:80` 포트 매핑을 사용한다.

## 종료 및 정리

### 실행 기록

```console
sarguments7021@cx2r7s4 codyssey-e1-1 % docker compose down
[+] Running 3/3
 ✔ Container codyssey-e1-1-probe-1  Removed                                                                                   0.0s 
 ✔ Container codyssey-e1-1-web-1    Removed                                                                                   0.2s 
 ✔ Network codyssey-e1-1_default    Removed
```

### 결과

- `docker compose down` 출력으로 `probe`·`web` 컨테이너와 Compose 기본 네트워크가 제거된 것을 확인했다.

## 환경 변수 활용 (.env 설정 후)

### 실행 기록

```console
sarguments7021@cx2r7s4 codyssey-e1-1 % docker compose config
name: codyssey-e1-1
services:
  probe:
    command:
      - wget
      - -qO-
      - http://web/
    depends_on:
      web:
        condition: service_started
        required: true
    image: busybox:1.36
    networks:
      default: null
  web:
    build:
      context: /Users/sarguments7021/codyssey-e1-1/src
      dockerfile: Dockerfile
    environment:
      APP_ENV: dev
    networks:
      default: null
    ports:
      - mode: ingress
        target: 80
        published: "8080"
        protocol: tcp
networks:
  default:
    name: codyssey-e1-1_default
sarguments7021@cx2r7s4 codyssey-e1-1 % docker compose up -d
[+] Running 3/3
 ✔ Network codyssey-e1-1_default    Created                                                                                   0.0s 
 ✔ Container codyssey-e1-1-web-1    Started                                                                                   0.2s 
 ✔ Container codyssey-e1-1-probe-1  Started                                                                                   0.2s 
sarguments7021@cx2r7s4 codyssey-e1-1 % docker compose exec web printenv APP_ENV
dev
sarguments7021@cx2r7s4 codyssey-e1-1 % curl -i http://localhost:8080
HTTP/1.1 200 OK
Server: nginx/1.31.3
Date: Fri, 31 Jul 2026 05:15:52 GMT
Content-Type: text/html
Content-Length: 302
Last-Modified: Fri, 31 Jul 2026 01:43:16 GMT
Connection: keep-alive
ETag: "6a6bfdb4-12e"
Accept-Ranges: bytes

<!doctype html>
<html lang="ko">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>E1-1 Docker Web Server</title>
  </head>
  <body>
    <h1>E1-1 Docker Web Server</h1>
    <p>Custom Nginx image is running.</p>
  </body>
</html>%
sarguments7021@cx2r7s4 codyssey-e1-1 % docker compose down
[+] Running 3/3
 ✔ Container codyssey-e1-1-probe-1  Removed                                                                                   0.0s 
 ✔ Container codyssey-e1-1-web-1    Removed                                                                                   0.2s 
 ✔ Network codyssey-e1-1_default    Removed                                                                                   0.1s 
```

VS Code에서 로컬 `.env`의 `APP_ENV`를 `dev`에서 `production`으로, `HOST_PORT`를 `8080`에서 `8082`로 변경했다.

```console
sarguments7021@cx2r7s4 codyssey-e1-1 % docker compose config
name: codyssey-e1-1
services:
  probe:
    command:
      - wget
      - -qO-
      - http://web/
    depends_on:
      web:
        condition: service_started
        required: true
    image: busybox:1.36
    networks:
      default: null
  web:
    build:
      context: /Users/sarguments7021/codyssey-e1-1/src
      dockerfile: Dockerfile
    environment:
      APP_ENV: production
    networks:
      default: null
    ports:
      - mode: ingress
        target: 80
        published: "8082"
        protocol: tcp
networks:
  default:
    name: codyssey-e1-1_default
sarguments7021@cx2r7s4 codyssey-e1-1 % docker compose up -d
[+] Running 3/3
 ✔ Network codyssey-e1-1_default    Created                                                                                   0.0s 
 ✔ Container codyssey-e1-1-web-1    Started                                                                                   0.1s 
 ✔ Container codyssey-e1-1-probe-1  Started                                                                                   0.2s 
sarguments7021@cx2r7s4 codyssey-e1-1 % docker compose exec web printenv APP_ENV
production
sarguments7021@cx2r7s4 codyssey-e1-1 % curl -i http://localhost:8082
HTTP/1.1 200 OK
Server: nginx/1.31.3
Date: Fri, 31 Jul 2026 05:16:37 GMT
Content-Type: text/html
Content-Length: 302
Last-Modified: Fri, 31 Jul 2026 01:43:16 GMT
Connection: keep-alive
ETag: "6a6bfdb4-12e"
Accept-Ranges: bytes

<!doctype html>
<html lang="ko">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>E1-1 Docker Web Server</title>
  </head>
  <body>
    <h1>E1-1 Docker Web Server</h1>
    <p>Custom Nginx image is running.</p>
  </body>
</html>%
sarguments7021@cx2r7s4 codyssey-e1-1 % docker compose down
[+] Running 3/3
 ✔ Container codyssey-e1-1-probe-1  Removed                                                                                   0.0s 
 ✔ Container codyssey-e1-1-web-1    Removed                                                                                   0.2s 
 ✔ Network codyssey-e1-1_default    Removed
```

### 결과

- 첫 번째 `docker compose config`는 `APP_ENV=dev`와 호스트 포트 `8080`을, 두 번째 출력은 `APP_ENV=production`과 호스트 포트 `8082`를 실제 Compose 설정으로 해석했다.
- `docker compose exec web printenv APP_ENV`의 `dev`와 `production` 출력은 Compose의 `environment` 설정이 이미지의 기본값을 실행 시점에 주입·덮어쓴 근거다.
- 두 `curl -i` 요청이 각각 `localhost:8080`, `localhost:8082`에서 `HTTP/1.1 200 OK`와 같은 커스텀 HTML을 반환했다. 즉 컨테이너 내부 Nginx 포트는 `80`으로 유지하면서 호스트 접속 포트만 설정으로 바꿨다.
- 두 번째 실행은 `docker compose up -d`만 사용했고 빌드 단계가 나타나지 않았다. 이미지와 HTML을 바꾸지 않고 Compose 설정만 바꾼 경우에는 재빌드 없이 새 실행 구성을 적용할 수 있다.
- 각 실행 뒤 `docker compose down`으로 서비스 컨테이너와 기본 네트워크를 정리했다.
