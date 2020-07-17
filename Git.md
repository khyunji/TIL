# Git

> Git은 분산형 버전 관리 시스템(DVCS)이다. Distributed Version Control System.
>
> 소스코드의 버전 및 이력을 관리할 수 있다.



## 준비하기

윈도우에서 Git을 활용하기 위해서 Git Bash를 설치한다. (MAC에는 기본적으로 Git이 깔려 있음!)

Git을 활용하기 위해서 GUI (<->CLI) 툴인 `source tree`, `github desktop`등을 활용할 수도 있다.

초기 설치를 완료한 이후에 컴퓨터에 author 정보를 입력한다.

```bash
$ git config --global user.name {user name}
$ git config --global user.email {user email}
```



## 로컬 저장소 (Repository) 활용하기

### 1. 저장소 초기화

```bash
$ git init
Initialized empty Git repository in C:/Users/kimsj/temp/.git/
```

- `.git` 폴더가 생성되며, 여기에 Git과 관련된 모든 정보가 저장된다.
- Git Bash에 `(master)` 라고 표시되는데, 이는 현재 폴더가 Git으로 관리되고 있다는 뜻이며 `master branch` 에 있다는 뜻이다.



### 2.  Add

`working directory` , 즉 작업 공간에서 변경된 사항을 이력으로 저장하기 위해서는 반드시 `staging area` 를 거쳐야 한다.

```bash
$ git add {파일/폴더}
$ git add markdown.md #파일을 staging area로 add
$ git add images/     #폴더 (이름 뒤에 /가 붙는 경우 폴더. /는 필수.)
$ git add .			  #현재 디렉토리(현재 작업하고 있는 폴더)에 있는 모든 파일과 폴더
```

- `add` 전 상태 (markdown.md 파일이 폴더에 새롭게 작성되었으나 아직 Git으로 관리되지 않고 add 되기 전 상태)

```bash
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        markdown.md

nothing added to commit but untracked files present (use "git add" to track)
```

- markdown.md 파일이 add 후에 staging area로 올라간 상태

```bash
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   markdown.md
```



### Commit

commit은 **이력을 확정짓는 명령어**로, 해당 시점의 스냅샷을 기록한다. (파일이나 폴더에 새로운 변경 사항이 있을 때  마지막 저장 상태에서부터 어떤 변화가 있었는지 기록하는 것.)

커밋 시에는 반드시 메시지를 작성해야 하며, 변경 사항을 알 수 있도록 명확하게 작성한다.

```bash
$ git commit -m "마크다운 정리"
[master (root-commit) 6c84015] 마크다운 정리
 1 file changed, 170 insertions(+)
 create mode 100644 markdown.md
```

커밋 이후에는 아래의 명령어를 통해 지금까지 작성된 커밋 이력을 확인한다.

```bash
$ git log
commit 6c840158f44a51f40c02f08d226289d899a6c904 (HEAD -> master)
Author: edujason-hphk <edujason.hphk@gmail.com>
Date:   Fri Jul 17 14:17:22 2020 +0900
    마크다운 정리
    
$ git log --oneline
6c84015 (HEAD -> master) 마크다운 정리
```

커밋은 해시값을 바탕으로 구분된다.



## 원격 저장소 (Remote Repository) 활용하기

원격 저장소 기능을 제공하는 다양한 서비스 중에 github을 기준으로 설명한다.

Github 생태계가 가장 크고, 다양한 open source repository들이 운영 중임.



### 0. 준비사항

- Github 회원가입
- Github에 repository 생성



## 1. 원격 저장소 등록

```bash
$ git remote add origin {github url}
```

- 원격저장소(remote)로 origin이라는 이름으로 Github url을 등록(add)한다.
- 등록된 원격 저장소를 보기 위해서는 아래의 명령어를 활용한다.

```bash
$ git remote -v
origin  https://github.com/kmhyunji/TIL.git (fetch)
origin  https://github.com/kmhyunji/TIL.git (push)
```



### 2. `push` - 원격 저장소로 업로드

```bash
$ git push origin master 
```

origin이라는 이름의 원격 저장소로 commit 기록들을 업로드한다.

이후 변경사항이 생길 때마다, `add - commit - push` 를 반복한다.