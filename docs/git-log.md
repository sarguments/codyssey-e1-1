# Git 및 GitHub 실습 로그

## Git 설정 및 GitHub 연동

### 실행 기록

```console
sarguments7021@c6r3s3 codyssey-e1-1 % git config --global user.name "jaehong"
sarguments7021@c6r3s3 codyssey-e1-1 % git config --global user.email "sarguments@gmail.com"
sarguments7021@c6r3s3 codyssey-e1-1 % git config --global init.defaultBranch main

sarguments7021@c6r3s3 codyssey-e1-1 % git config --list
credential.helper=osxkeychain
user.name=jaehong
user.email=sarguments@gmail.com
init.defaultbranch=main
core.repositoryformatversion=0
core.filemode=true
core.bare=false
core.logallrefupdates=true
core.ignorecase=true
core.precomposeunicode=true
remote.origin.url=https://github.com/sarguments/codyssey-e1-1.git
remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
branch.main.remote=origin
branch.main.merge=refs/heads/main
branch.main.vscode-merge-base=origin/main

sarguments7021@c6r3s3 codyssey-e1-1 % git remote -v
origin  https://github.com/sarguments/codyssey-e1-1.git (fetch)
origin  https://github.com/sarguments/codyssey-e1-1.git (push)
```

### 결과

- `git config --list`에서 사용자 정보와 기본 브랜치가 `main`으로 설정된 것을 확인했다.
- `git remote -v`에 GitHub 저장소 주소가 표시됐다.

### Git과 GitHub

- Git은 로컬에서 변경 이력을 기록하고 관리하는 버전 관리 도구다.
- GitHub는 Git 저장소를 원격으로 공유하고 협업 기능을 제공하는 플랫폼이다.

### GitHub 및 VS Code 연동 증거

[![GitHub 및 VS Code 연동 증거](../evidence/vscode-github.png)](../evidence/vscode-github.png)

## GitHub SSH 키 설정

### 기존 키 및 접속 확인

#### 실행 기록

```console
sarguments7021@cx2r7s4 codyssey-e1-1 % ls -la ~/.ssh
total 8
drwxr-xr-x   3 sarguments7021  sarguments7021   96 Jul 31 10:33 .
drwxr-x---+ 27 sarguments7021  sarguments7021  864 Jul 31 13:51 ..
-rw-r--r--   1 sarguments7021  sarguments7021  210 Jul 31 10:33 config
sarguments7021@cx2r7s4 codyssey-e1-1 % ssh -T git@github.com
The authenticity of host 'github.com (20.200.245.247)' can't be established.
ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? y
Please type 'yes', 'no' or the fingerprint: y
Please type 'yes', 'no' or the fingerprint: yes
Warning: Permanently added 'github.com' (ED25519) to the list of known hosts.
git@github.com: Permission denied (publickey).
```

#### 결과

- `~/.ssh` 목록에는 설정 파일만 있었고, 이 디렉터리에 기존 개인키·공개키 쌍은 없었다.
- `yes` 입력 후 GitHub 호스트 키는 `known_hosts`에 등록됐지만, 계정에 사용할 공개키가 없어 `Permission denied (publickey)`로 인증에 실패했다.

### 키 생성 및 계정 인증

#### 실행 기록

```console
sarguments7021@cx2r7s4 codyssey-e1-1 % ssh-keygen -t ed25519 -C "sarguments@gmail.com" -f ~/.ssh/id_ed25519
Generating public/private ed25519 key pair.
Enter passphrase for "/Users/sarguments7021/.ssh/id_ed25519" (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /Users/sarguments7021/.ssh/id_ed25519
Your public key has been saved in /Users/sarguments7021/.ssh/id_ed25519.pub
The key fingerprint is:
SHA256:8iJzzvuqPp2Opiwa2NRBxHPfRpQw3ZNk3NeWmMYrljg sarguments@gmail.com
The key's randomart image is:
+--[ED25519 256]--+
|   oo   o+.=+oo o|
|   .o .  .+.==..+|
|    .o . o. o..o |
|   . .  .Eo+ .   |
|  . . . S.o .    |
|.o     o         |
|o . o.o..        |
|.o  oBo.         |
|o.o+++*+.        |
+----[SHA256]-----+
sarguments7021@cx2r7s4 codyssey-e1-1 % eval "$(ssh-agent -s)"
Agent pid 79723
sarguments7021@cx2r7s4 codyssey-e1-1 % /usr/bin/ssh-add --apple-use-keychain ~/.ssh/id_ed25519
Identity added: /Users/sarguments7021/.ssh/id_ed25519 (sarguments@gmail.com)
sarguments7021@cx2r7s4 codyssey-e1-1 % pbcopy < ~/.ssh/id_ed25519.pub
sarguments7021@cx2r7s4 codyssey-e1-1 % ssh -T git@github.com
Hi sarguments! You've successfully authenticated, but GitHub does not provide shell access.
```

#### 결과

- 개인키를 `ssh-agent`와 macOS Keychain에 등록한 뒤 공개키를 GitHub 계정에 등록했다.
- 마지막 `ssh -T`의 인증 성공 메시지로 GitHub가 이 키를 `sarguments` 계정의 인증 키로 받아들인 것을 확인했다.

### SSH 원격 전환 및 Git 검증

#### 실행 기록

```console
sarguments7021@cx2r7s4 codyssey-e1-1 % git remote set-url origin git@github.com:sarguments/codyssey-e1-1.git
sarguments7021@cx2r7s4 codyssey-e1-1 % git remote -v
origin  git@github.com:sarguments/codyssey-e1-1.git (fetch)
origin  git@github.com:sarguments/codyssey-e1-1.git (push)
sarguments7021@cx2r7s4 codyssey-e1-1 % git ls-remote --heads origin main
65c6dbde42df320fa3c697cb65320eef1161c0bf        refs/heads/main
sarguments7021@cx2r7s4 codyssey-e1-1 % git push --dry-run origin main
Everything up-to-date
```

#### 결과

- `origin`의 fetch·push 주소가 모두 `git@github.com:sarguments/codyssey-e1-1.git`인 것을 확인했다.

### SSH 원격과 PAT·키 보안

- HTTPS와 SSH 모두 통신을 암호화한다. 차이는 인증 방식으로, HTTPS는 PAT(Personal Access Token)를 비밀번호 대신 쓰고 SSH는 개인키·공개키 쌍을 쓴다.

| 구분 | HTTPS + PAT | SSH 키 |
| --- | --- | --- |
| 원격 주소 예 | `https://github.com/OWNER/REPO.git` | `git@github.com:OWNER/REPO.git` |
| 인증 수단 | PAT 값을 비밀번호 대신 사용 | 개인키로 인증 요청에 서명 |
| 로컬 보관 | macOS Keychain 같은 자격증명 저장소 | 개인키와 `ssh-agent`·Keychain |
| 주의할 비밀 | PAT 값 | 개인키 |

- SSH에서는 GitHub에 공개키만 등록하고 개인키는 Mac에 둔다. 공개키만으로 개인키를 현실적으로 알아낼 수 없고, 인증할 때도 개인키 자체를 보내지 않고 현재 연결에 대한 서명만 검증하므로 안전하다.