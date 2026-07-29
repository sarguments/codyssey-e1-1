# 터미널 및 권한 실습 로그

## 터미널 조작

- 요구사항: 현재 위치 확인, 숨김 파일을 포함한 목록 확인, 디렉터리 이동, 파일 생성·복사·이름 변경·삭제, 파일 내용 확인, 빈 파일 생성을 기록한다.

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

#### 확인 기준

- 첫 번째 `pwd`가 시작 위치를, `cd` 뒤의 `pwd`가 이동한 위치를 출력하는지 확인한다.
- `ls -la`에 숨김 파일을 포함한 목록이 표시되는지 확인한다.
- 각 목록에서 생성·복사·이름 변경·삭제에 따른 파일 상태 변화를 확인한다.

#### 설명

- 절대 경로는 `/`부터 시작해 현재 작업 디렉터리와 관계없이 같은 위치를 가리킨다.
- 상대 경로는 현재 작업 디렉터리를 기준으로 해석된다.
- 예시: `practice/terminal`에서 `workspace/original.txt`는 상대 경로다. 두 번째 `pwd` 출력 뒤에 `/workspace/original.txt`를 붙인 값은 같은 파일의 절대 경로다.

#### 파일 상태 변화

- `mkdir -p workspace` 뒤: 빈 `workspace/` 디렉터리가 생긴다.
- `printf ... > workspace/original.txt` 뒤: `original.txt`가 생기고 `terminal practice` 내용이 저장된다.
- `cp ... copy.txt` 뒤: `original.txt`는 유지되고, 같은 내용을 가진 `copy.txt`가 추가된다.
- `mv copy.txt renamed.txt` 뒤: `copy.txt`는 없어지고 `renamed.txt`가 생긴다.
- `rm renamed.txt` 뒤: `renamed.txt`가 삭제되고 `original.txt`만 남는다.

#### `printf` 명령 설명

- `printf`는 지정한 문자열을 표준 출력으로 보낸다.
- `>`는 그 출력을 파일로 보내며, 같은 이름의 파일이 있으면 기존 내용을 덮어쓴다.
- `\n`은 줄바꿈을 뜻한다. 따라서 이 실습에서는 `original.txt`에 `terminal practice` 한 줄을 만든다.
- 기존 파일 끝에 내용을 추가하려면 `>` 대신 `>>`를 사용한다.

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

#### 확인 기준

- 파일 내용 확인 명령이 작성한 내용을 출력하는지 확인한다.
- 빈 파일 생성 뒤 `ls -l` 또는 동등 명령에서 파일이 존재하고 크기가 0인지 확인한다.

## 권한 실습

- 요구사항: 파일 1개와 디렉터리 1개에 대해 권한 변경 전후를 기록한다.

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

#### 확인 기준

- 변경 전과 후의 `ls -l` 권한 표기가 다른지 확인한다.
- 변경 후 권한이 지정한 값과 일치하는지 확인한다.

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

#### 확인 기준

- 변경 전과 후의 `ls -ld` 권한 표기가 다른지 확인한다.
- 변경 후 권한이 지정한 값과 일치하는지 확인한다.

### 권한 표기 설명

- `ls -l`과 `ls -ld` 출력의 첫 글자는 권한이 아니라 항목 종류다.
  - `-`는 일반 파일이다. 예: `-rw-r--r--`
  - `d`는 디렉터리다. 예: `drwxr-xr-x`
  - `ls -ld permissions-dir`의 `-d`는 디렉터리 안의 목록이 아니라 `permissions-dir` 자체의 정보를 보라는 옵션이다.
- 첫 글자 뒤의 아홉 글자가 권한이며, 소유자·그룹·기타 사용자 순서로 세 글자씩 나뉜다.
  - `drwx------`은 `d | rwx | --- | ---`이므로 소유자만 접근할 수 있다.
  - `drwxr-xr-x`은 `d | rwx | r-x | r-x`이므로 그룹과 기타 사용자도 목록을 보고 디렉터리에 들어갈 수 있지만, 내용을 바꾸지는 못한다.
- `r`, `w`, `x`는 각각 읽기, 쓰기, 실행 권한이다. 디렉터리의 `x`는 해당 디렉터리에 들어가거나 내부 항목에 접근할 수 있는 권한과 관련된다.
- 세 자리 숫자는 왼쪽부터 소유자, 그룹, 기타 사용자의 권한을 뜻한다.
- 각 자리는 `rwx` 세 권한을 나타내는 비트 세 개를 8진수 한 자리로 적은 것이다.
  - 읽기 권한은 2진수 `100`이므로 `4`다.
  - 쓰기 권한은 2진수 `010`이므로 `2`다.
  - 실행 권한은 2진수 `001`이므로 `1`이다.
- 각 권한의 값을 더해 숫자를 만든다.
  - `7 = 4 + 2 + 1`이므로 `rwx`다.
  - `6 = 4 + 2`이므로 `rw-`다.
  - `5 = 4 + 1`이므로 `r-x`다.
  - `4`는 읽기 권한만 있으므로 `r--`다.
- `755`는 소유자 `rwx`, 그룹 `r-x`, 기타 사용자 `r-x`이다.
- `644`는 소유자 `rw-`, 그룹 `r--`, 기타 사용자 `r--`이다.
