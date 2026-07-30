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
