# 터미널 및 권한 실습 로그

## 터미널 조작

### 파일·디렉터리 조작

#### 실행 기록

```console
sarguments7021@c6r3s3 codyssey-e1-1 % pwd
/Users/sarguments7021/codyssey-e1-1

sarguments7021@c6r3s3 codyssey-e1-1 % ls -al
total 32
drwxr-xr-x   9 sarguments7021  sarguments7021    288 Jul 29 16:15 .
drwxr-x---+ 24 sarguments7021  sarguments7021    768 Jul 29 15:01 ..
drwxr-xr-x  12 sarguments7021  sarguments7021    384 Jul 29 14:57 .git
-rw-r--r--   1 sarguments7021  sarguments7021     49 Jul 29 14:57 .gitignore
drwxr-xr-x   6 sarguments7021  sarguments7021    192 Jul 29 16:38 docs
drwxr-xr-x   3 sarguments7021  sarguments7021     96 Jul 29 14:57 evidence
drwxr-xr-x   5 sarguments7021  sarguments7021    160 Jul 29 16:40 practice
-rw-r--r--   1 sarguments7021  sarguments7021  10386 Jul 29 16:45 README.md
drwxr-xr-x   6 sarguments7021  sarguments7021    192 Jul 29 16:15 src

sarguments7021@c6r3s3 codyssey-e1-1 % cd practice/terminal
sarguments7021@c6r3s3 terminal % pwd
/Users/sarguments7021/codyssey-e1-1/practice/terminal

sarguments7021@c6r3s3 terminal % mkdir -p workspace
sarguments7021@c6r3s3 terminal % ls -al workspace
total 0
drwxr-xr-x  2 sarguments7021  sarguments7021   64 Jul 29 16:57 .
drwxr-xr-x  4 sarguments7021  sarguments7021  128 Jul 29 16:57 ..

sarguments7021@c6r3s3 terminal % printf 'terminal practice\n' > workspace/original.txt
sarguments7021@c6r3s3 terminal % ls -al workspace
total 8
drwxr-xr-x  3 sarguments7021  sarguments7021   96 Jul 29 16:57 .
drwxr-xr-x  4 sarguments7021  sarguments7021  128 Jul 29 16:57 ..
-rw-r--r--  1 sarguments7021  sarguments7021   18 Jul 29 16:57 original.txt

sarguments7021@c6r3s3 terminal % cp workspace/original.txt workspace/copy.txt
sarguments7021@c6r3s3 terminal % ls -al workspace
total 16
drwxr-xr-x  4 sarguments7021  sarguments7021  128 Jul 29 16:58 .
drwxr-xr-x  4 sarguments7021  sarguments7021  128 Jul 29 16:57 ..
-rw-r--r--  1 sarguments7021  sarguments7021   18 Jul 29 16:58 copy.txt
-rw-r--r--  1 sarguments7021  sarguments7021   18 Jul 29 16:57 original.txt

sarguments7021@c6r3s3 terminal % mv workspace/copy.txt workspace/renamed.txt
sarguments7021@c6r3s3 terminal % ls -al workspace
total 16
drwxr-xr-x  4 sarguments7021  sarguments7021  128 Jul 29 16:59 .
drwxr-xr-x  4 sarguments7021  sarguments7021  128 Jul 29 16:57 ..
-rw-r--r--  1 sarguments7021  sarguments7021   18 Jul 29 16:57 original.txt
-rw-r--r--  1 sarguments7021  sarguments7021   18 Jul 29 16:58 renamed.txt

sarguments7021@c6r3s3 terminal % rm workspace/renamed.txt
sarguments7021@c6r3s3 terminal % ls -al workspace
total 8
drwxr-xr-x  3 sarguments7021  sarguments7021   96 Jul 29 17:00 .
drwxr-xr-x  4 sarguments7021  sarguments7021  128 Jul 29 16:57 ..
-rw-r--r--  1 sarguments7021  sarguments7021   18 Jul 29 16:57 original.txt
```

#### 결과

- `pwd`로 저장소 루트와 `practice/terminal`로 이동한 뒤의 위치를 확인했다.
- `ls -al`에서 `.git`, `.gitignore` 등 숨김 항목까지 확인했다.
- `original.txt`를 생성한 뒤 복사, 이름 변경, 삭제한 결과가 각 목록에 반영됐다.

#### 경로 정리

- 절대 경로는 `/`부터 시작해 현재 작업 디렉터리와 관계없이 같은 위치를 가리킨다.
- 상대 경로는 현재 작업 디렉터리를 기준으로 해석된다.
- 이 실습에서 `workspace/original.txt`는 상대 경로이고, 같은 파일의 절대 경로는 `/Users/sarguments7021/codyssey-e1-1/practice/terminal/workspace/original.txt`다.

### 파일 내용과 빈 파일

#### 실행 기록

```console
sarguments7021@c6r3s3 codyssey-e1-1 % cd practice/terminal
sarguments7021@c6r3s3 terminal % cat workspace/original.txt
terminal practice

sarguments7021@c6r3s3 terminal % touch workspace/empty.txt
sarguments7021@c6r3s3 terminal % ls -l workspace/empty.txt
-rw-r--r--  1 sarguments7021  sarguments7021  0 Jul 29 17:02 workspace/empty.txt
```

#### 결과

- `cat`으로 `original.txt`의 내용을 확인했다.
- `touch`로 만든 `empty.txt`는 크기가 `0`으로 표시됐다.

## 권한 실습

### 파일 권한

#### 실행 기록

```console
sarguments7021@c6r3s3 codyssey-e1-1 % cd practice/permissions
sarguments7021@c6r3s3 permissions % touch permissions-file.txt
sarguments7021@c6r3s3 permissions % chmod 600 permissions-file.txt
sarguments7021@c6r3s3 permissions % ls -l permissions-file.txt
-rw-------  1 sarguments7021  sarguments7021  0 Jul 29 17:04 permissions-file.txt

sarguments7021@c6r3s3 permissions % chmod 644 permissions-file.txt
sarguments7021@c6r3s3 permissions % ls -l permissions-file.txt
-rw-r--r--  1 sarguments7021  sarguments7021  0 Jul 29 17:04 permissions-file.txt
```

#### 결과

- `chmod 600`에서는 `-rw-------`, `chmod 644`에서는 `-rw-r--r--`로 표시됐다.

### 디렉터리 권한

#### 실행 기록

```console
sarguments7021@c6r3s3 codyssey-e1-1 % cd practice/permissions
sarguments7021@c6r3s3 permissions % mkdir -p permissions-dir
sarguments7021@c6r3s3 permissions % chmod 700 permissions-dir
sarguments7021@c6r3s3 permissions % ls -ld permissions-dir
drwx------  2 sarguments7021  sarguments7021  64 Jul 29 17:05 permissions-dir

sarguments7021@c6r3s3 permissions % chmod 755 permissions-dir
sarguments7021@c6r3s3 permissions % ls -ld permissions-dir
drwxr-xr-x  2 sarguments7021  sarguments7021  64 Jul 29 17:05 permissions-dir
```

#### 결과

- `chmod 700`에서는 `drwx------`, `chmod 755`에서는 `drwxr-xr-x`로 표시됐다.

### 권한 표기

- 첫 글자의 `-`는 일반 파일, `d`는 디렉터리를 뜻한다. 뒤의 아홉 글자는 소유자·그룹·기타 사용자의 권한이다.
- `r`은 읽기, `w`는 쓰기, `x`는 실행 권한이다. 디렉터리의 `x`는 내부 항목에 접근할 수 있는 권한이다.
- 숫자 표기에서는 `r=4`, `w=2`, `x=1`을 더한다.
- `644`는 `rw-r--r--`, `755`는 `rwxr-xr-x`다.
