# < Git Flow Branch 전략 >







# 1. Git Flow Branch 전략



## 1) Git FLow 개요



Vincent Driessen 란 분이 2010년에 블로그에 올린 글에 의해 널리 퍼지기 시작한 git branch 관리 방법이다.

특별한 기술은 아니고, 협업하는 사람들끼리 브랜치 관리에 대해 "우리 이렇게 브랜치 전략 짜자" 와 같은 방법론(Model)일 뿐입니다.





## 2) git flow 브랜치와 그 의미

그럼, 각각의 브랜치와 의미에 대해 알아보겠습니다.



Git-flow에는 5가지 종류의 브랜치가 존재한다.

 항상 유지되는 메인 브랜치들(master, develop)과 일정 기간 동안만 유지되는 보조 브랜치들(feature, release, hotfix)이 있다.



- master : 제품으로 출시될 수 있는 브랜치
- develop : 다음 출시 버전을 개발하는 브랜치
  - 이 브랜치를 기준으로 feature 브랜치를 따고, 각 feature를 합치는 브랜치
- feature : 단위 기능을 개발하는 브랜치
- release : 이번 출시 버전에 기능에 문제가 없는지 품질체크(QA) 용도의 브랜치
- hotfix : 출시 버전에서 발생한 버그를 수정 하는 브랜치





**master** : 소프트웨어 제품을 배포하는 용도로 쓰는 브랜치

**develop** : 개발용 default 브랜치로, 이 브랜치를 기준으로 feature 브랜치를 따고, 각 feature를 합치는 브랜치

**feature:** 단위 기능 개발용 브랜치

**release:** 다음배포를 위해 기능에 문제가 없는지 품질체크(QA) 용도의 브랜치

**hotfix:** 배포가 되고 나서(master에 배포 코드가 합쳐진 후) 버그 발생 시 긴급 수정하는 브랜치

**support:** 버전 호환성을 위한 브랜치













## 3) git flow

![img](gitflow-branch.assets/git-model@2x.png)



1. master 에서 시작
2. master가 base인 develop 브랜치 생성
3. 개발자1 : develop이 base인 feature 브랜치를 생성하여 개발 진행

  3-1. 개발자2 : develop이 base인 feature 브랜치를 생성하여 개발 진행

  ...

4. 개발 완료된 feature 브랜치는 develop으로 merge
5. release 나갈 브랜치를 develop base 에서 생성
6. release branch에 있는 코드에 대해 QA를 진행하면서 버그를 고쳐나감.
7. QA 통과한 release branch는 이제 배포 준비 완료된 상태
8. 배포를 위해 release branch -> develop, master로 합침
9. master 브랜치에서도 각 코드 버전에 대한 기록을 남기기 위해 태그도 추가로 생성
10. 보통은 이렇게 생성된 태그로 배포
11. 만약 배포 나간 건에 대해서 긴급히 버그 처리해야할 경우 master base 기반으로 hotfix 브랜치 생성
12. hotfix 브랜치를 master, develop에 머지





## 4) git flow 상세



### develop Branch

git flow 에서 가장 중심에 있는 브랜치





### feature Branch

* feature가 커지면 좋지 않다.
  * Conflict방지
* 



### git flow 에서 Branch 이름

```
master
develop
feature/{name}
release/{name}
hotfix/{name}
```

git flow 를 쉽게 사용할 수 있도록 제공하는 플러그인도 이런 모습이다.



### release branch

release branch는 QA를 받는 용도이다.

git flow의 release 브랜치는 다음과 같이 설명되어 있습니다.



*  develop에서 release 브랜치가 생성되는 순간 부터 develop 코드가 다음 release 를 위한 개발을 시작할 수 있다.
* **release/{name} 으로 생성시  {name}이 tag명으로 명명 된다.
  * 예를들면
    * release/v1.0.5 로 작업후 release finish 하게 되면 v1.0.5 라는 tag 가 만들어 진다.
  * hotfix 도 동일하다.





### Branch / Merge 흐름





![gitflow release branch](gitflow-branch.assets/gitflow-release-branch.jpg)



* Git flow Release Branch는 Develop Branch에서 생성됨
* finish 되면 Master 와 Develop Branch로 Merge된다.









## 5) git & git flow install

```sh

$ sudo apt-get install git

$ sudo apt-get install git-flow



# 버젼 확인

$ git version
git version 2.34.1


$ git flow version
1.12.3 (AVH Edition)

```







## 6) git flow 실습1

아무런 데이터 없이 그냥 git flow 명령들을 실행해 보면서 브랜치 움직임을 살펴보자.



### git flow init

```sh
$ mkdir -p ~/temp/gitflowtest
  cd ~/temp/gitflowtest



# 초기 기본Branch 설정
$ git config --global init.defaultBranch master


$ git flow init

Initialized empty Git repository in /home/ktdseduuser/song/gittest/gitflow/.git/
No branches exist yet. Base branches must be created now.
Branch name for production releases: [master]          # <-- 어떤 브랜치를 production용으로 사용할 것인지
Branch name for "next release" development: [develop]  # <-- 다음 릴리즈 브랜치는 어디 기반으로 할 것인지

How to name your supporting branch prefixes?
Feature branches? [feature/]
Bugfix branches? [bugfix/]
Release branches? [release/]
Hotfix branches? [hotfix/]
Support branches? [support/]
Version tag prefix? []
Hooks and filters directory? [/home/ktdseduuser/song/gittest/gitflow/.git/hooks]


```

* 어떤 브랜치를 production용으로 사용할 것인지, 
* 다음 릴리즈 브랜치는 어디 기반으로 할 것인지(이게 위에서 말한 release 브랜치를 생성할 때 베이스 브랜치를 어디로 잡을지를 의미) 
* 위에 설명할 때 feautre/{name}, release/{name}, hotfix/{name} 이라고 했는데, 디폴트로 저런 형태를 많이 사용한다는 의미
* 바꾸고 싶으면 바꿔도 된다는 의미





### feature start

```sh

$ git flow feature start gitflow_feature_branch
Switched to a new branch 'feature/gitflow_feature_branch'

Summary of actions:
- A new branch 'feature/gitflow_feature_branch' was created, based on 'develop'
- You are now on branch 'feature/gitflow_feature_branch'

Now, start committing on your feature. When done, use:

     git flow feature finish gitflow_feature_branch


# branch 확인
$ git branch -a
  develop
* feature/gitflow_feature_branch
  master

```



### feature  finish

Git flow feature 가 완료되면 feature branch 가 삭제된다.

```sh
$ git flow feature finish gitflow_feature_branch
Switched to branch 'develop'
Already up to date.
Deleted branch feature/gitflow_feature_branch (was b63b3b4).

Summary of actions:
- The feature branch 'feature/gitflow_feature_branch' was merged into 'develop'
- Feature branch 'feature/gitflow_feature_branch' has been locally deleted
- You are now on branch 'develop'



# 확인
$ git branch -a

* develop
  master

```



### release start

```sh
$ git flow release start 0.2.1
Switched to a new branch 'release/0.2.1'

Summary of actions:
- A new branch 'release/0.2.1' was created, based on 'develop'
- You are now on branch 'release/0.2.1'

Follow-up actions:
- Bump the version number now!
- Start committing last-minute fixes in preparing your release
- When done, run:

     git flow release finish '0.2.1'


$ git branch -a
  develop
  master
* release/0.2.1

```

* release/0.2.1 브랜치를 기반으로 Test 를 수행
* 상황에 따라서는 수정 개발(bugfix)도 수행할 수 있다.
* 필요한 경우 bugfix 건을 develop 브랜체 반영(merge)해야 한다.
* 이때 develop 브랜치는 다음 release를 위한 개발을 수행한다.





### release finish

release Branche작업 완료시 finish 명령을 사용한다.

```sh
$ git flow release finish '0.2.1'

Switched to branch 'master'
Deleted branch release/0.2.1 (was b63b3b4).

Summary of actions:
- Release branch 'release/0.2.1' has been merged into 'master'
- The release was tagged '0.2.1'
- Release branch 'release/0.2.1' has been locally deleted
- You are now on branch 'master'

$ git branch -a
  develop
* master


$ git tag -l
0.2.1

```

release Branch 가 삭제되고 tag 와 develop / master Branch 만 남는다.







## 7) git flow 실습2



실제 데이터를 이용하여 git flow 를 수행해 보자.

git flow 를 이해하기 위함이므로 remote 연결없이 local 에서만 수행한다.



### Sample Data 생성

#### 디렉토리 초기화

```sh

$ rm -rf ~/temp/gitflowtest
  mkdir ~/temp/gitflowtest
  cd ~/temp/gitflowtest

```



#### Data 생성

개발 디렉토리에는 기능 A 와 기능 B가 있다고 가정한다.

```sh

# Sample Data file 생성
$ echo aaa > A
  echo bbb > B

```





### git init

git init 이후에는 반드시 초기 commit 을 수행해야 한다. 

```sh

# 초기 기본Branch 설정
$ git config --global init.defaultBranch master
  git config --global user.email "ssongmantop@gmail.com"
  git config --global user.name "root"

$ git init


$ git add -A

$ git commit -m "update init"


$ git branch -a
* master


```



### [참고] git init 작업 삭제시

```sh

# git flow init 작업을 삭제하려면...
$ rm -rf .git

```





### git flow init

```sh

$ git flow init

Which branch should be used for bringing forth production releases?
   - master
Branch name for production releases: [master]
Branch name for "next release" development: [develop]

...


# 확인
$ git branch -a
* develop
  master


```





### feature start

* 기능 개발 시작
  * A 파일을 삭제

```sh

$ git flow feature start test

Switched to a new branch 'feature/test'

Summary of actions:
- A new branch 'feature/test' was created, based on 'develop'
- You are now on branch 'feature/test'

Now, start committing on your feature. When done, use:

     git flow feature finish test

```

feature branch는 develop branch 를 기반으로 생성됨

```sh

# branch 확인
~/temp/gitflowtest(feature/test)$ git branch -a
  develop
* feature/test
  master

```



A 파일 삭제

```sh

~/temp/gitflowtest(feature/test)$ ll
-rw-rw-r-- 1 ktdseduuser ktdseduuser    4 Sep 20 10:10 A
-rw-rw-r-- 1 ktdseduuser ktdseduuser    4 Sep 20 10:10 B


~/temp/gitflowtest(feature/test)$ rm A

~/temp/gitflowtest(feature/test)$ ll
-rw-rw-r-- 1 ktdseduuser ktdseduuser    4 Sep 20 10:10 B


~/temp/gitflowtest(feature/test)$ git status
On branch feature/test
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        deleted:    .gitignore
        deleted:    A

# add & commit
~/temp/gitflowtest(feature/test)$ git add -A

~/temp/gitflowtest(feature/test)$ git commit -m "delete A"
[feature/test 7c1d3a1] delete A
 2 files changed, 2 deletions(-)
 delete mode 100644 .gitignore
 delete mode 100644 A




~/temp/gitflowtest(feature/test)$ git status

On branch feature/test
nothing to commit, working tree clean


```



### feature finish

```sh

~/temp/gitflowtest(feature/test)$ git flow feature finish test

Switched to branch 'develop'
Updating daf8b40..7c1d3a1
Fast-forward
 .gitignore | 1 -
 A          | 1 -
 2 files changed, 2 deletions(-)
 delete mode 100644 .gitignore
 delete mode 100644 A
Deleted branch feature/test (was 7c1d3a1).

Summary of actions:
- The feature branch 'feature/test' was merged into 'develop'
- Feature branch 'feature/test' has been locally deleted
- You are now on branch 'develop'

~/temp/gitflowtest(develop)$


~/temp/gitflowtest(develop)$ git branch -a
* develop
  master

~/temp/gitflowtest(develop)$ ll
-rw-rw-r-- 1 ktdseduuser ktdseduuser    4 Sep 20 10:10 B



```





### release start

배포 준비를 위한 Release 브랜치생성

```sh

~/temp/gitflowtest(develop)$ git flow release start 1.0.0
Switched to a new branch 'release/1.0.0'

Summary of actions:
- A new branch 'release/1.0.0' was created, based on 'develop'
- You are now on branch 'release/1.0.0'

Follow-up actions:
- Bump the version number now!
- Start committing last-minute fixes in preparing your release
- When done, run:

     git flow release finish '1.0.0'


~/temp/gitflowtest(release/1.0.0)$ git branch -a
  develop
  master
* release/1.0.0


```



갑자기 기능 C 를 추가해 달라는 요청이 급하게 왔다.

이때 기능 C 를 1.0.0 에 포함하여 배포할것인지 다음 release 에 포함시킬 것인지 결정해야 한다.

이 결정에 따라 처리 방법이 틀려진다.

만약 1.0.0 에 배포되어야 한다면 현재 release/1.0.0 브랜치에서 작업을 진행하여 차후 develop 브랜치에 merge 된다.

하지만 다음 release 때 배포되어야 한다면 새로운 feature 브랜치를 생성(병렬 구조로 개발) 시켜야 한다.

본 실습에서는 1.0.0에 포함시키도록 하자.



```sh

# 기능 C 추가

~/temp/gitflowtest(release/1.0.0)$ echo ddd > C

~/temp/gitflowtest(release/1.0.0)$ ll
-rw-rw-r-- 1 ktdseduuser ktdseduuser    4 Sep 20 10:10 B
-rw-rw-r-- 1 ktdseduuser ktdseduuser    4 Sep 20 10:10 C
-rw-rw-r-- 1 ktdseduuser ktdseduuser    4 Sep 20 13:57 C

~/temp/gitflowtest(release/1.0.0)$ git add -A

~/temp/gitflowtest(release/1.0.0)$ git commit -m "C added"
[release/1.0.0 e20c05c] C added
 1 file changed, 1 insertion(+)
 create mode 100644 C

```



테스트 작업 수행

```

...
테스트 작업 수행
...

```





### release finish

```sh
$ git flow release finish 1.0.0

Switched to branch 'master'
Merge made by the 'ort' strategy.
 .gitignore | 1 -
 A          | 1 -
 C          | 1 +
 3 files changed, 1 insertion(+), 2 deletions(-)
 delete mode 100644 .gitignore
 delete mode 100644 A
 create mode 100644 C
Already on 'master'
Switched to branch 'develop'
Merge made by the 'ort' strategy.
 C | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 C
Deleted branch release/1.0.0 (was e20c05c).

Summary of actions:
- Release branch 'release/1.0.0' has been merged into 'master'
- The release was tagged '1.0.0'
- Release tag '1.0.0' has been back-merged into 'develop'
- Release branch 'release/1.0.0' has been locally deleted
- You are now on branch 'develop'


```

release finish 때 진행하는 작업을 살펴보자

* master merge
  * 기능 A삭제건과 C 추가건을 master 에 merge 시킨다.
* develop merge
  * 기능 C 추가건을 merge 한다.
* Tag
  * 1.0.0 으로 tag 를 남긴다.
* release/1.0.0브랜치를 삭제한다.
* 현재 브랜치를 develop 으로 변경한다.



브랜치와 새롭게 생성된 tag를 확인해보자.

```sh

~/temp/gitflowtest(develop)$ git branch -a
* develop
  master


 ~/temp/gitflowtest(develop)$ git tag -l
1.0.0


```

* release Branch 가 삭제되고 tag 와 develop / master Branch 만 남는다.

* 최종 배포시에는 tag 1.0.0 을 배포한다.





### hotfix start

1.0.0 을 배포후 확인해 보니 기능D 가 급하게 추가 되어야 하는 상황이 발생했다. 

hotfix 브랜치를 생성해야 한다.

hotfix 브랜치는 production 인 master branch 를 기반으로 생성된다.



```sh

$ git flow hotfix start 1.0.1

Switched to a new branch 'hotfix/1.0.1'

Summary of actions:
- A new branch 'hotfix/1.0.1' was created, based on 'master'
- You are now on branch 'hotfix/1.0.1'

Follow-up actions:
- Start committing your hot fixes
- Bump the version number now!
- When done, run:

     git flow hotfix finish '1.0.1'


~/temp/gitflowtest(hotfix/1.0.1)$
 
 

### 기능 D 추가

~/temp/gitflowtest(hotfix/1.0.1)$ echo ddd > D
 
~/temp/gitflowtest(hotfix/1.0.1)$ ll
-rw-rw-r-- 1 ktdseduuser ktdseduuser    4 Sep 20 10:10 B
-rw-rw-r-- 1 ktdseduuser ktdseduuser    4 Sep 20 10:10 C
-rw-rw-r-- 1 ktdseduuser ktdseduuser    4 Sep 20 15:31 D


# add & commit
~/temp/gitflowtest(hotfix/1.0.1)$ git add .
~/temp/gitflowtest(hotfix/1.0.1)$ git commit -m "create D"

```

* 긴급 개발후 테스트까지 종료했다.







### hotfix finish

```sh

# hotfix finish
~/temp/gitflowtest(hotfix/1.0.1)$ git flow hotfix finish 1.0.1
Switched to branch 'develop'
Your branch is up to date with 'origin/develop'.
Deleted branch hotfix/1.0.1 (was 25f9958).

Summary of actions:
- Hotfix branch 'hotfix/1.0.1' has been merged into 'master'
- The hotfix was tagged '1.0.1'
- Hotfix branch 'hotfix/1.0.1' has been locally deleted
- You are now on branch 'develop'

```

* release finish 작업 절차와 유사하다.
  * hotfix 브랜치 코드를 master, develop 으로 merge
  * hotfix 브랜치 name 으로 tag 생성
  * local hotfix 브랜치 삭제



현재까지는 local 에서만 진행된 상황이며 이를 orgin 에 업데이트 하려면 git push를 수행해야 한다.

```sh

$ git push origin master develop 1.0.0 1.0.1

```











## 8) git flow 실습3



git flow 실습자료를 remote 와 연결해 보자.



### git remote 



```sh


$ cd ~/temp/gitflowtest


$ git checkout master
 
$ git remote add origin http://gitlab.35.209.207.26.nip.io/cloud-branch/gitflow-test/edu01.git

$ git remote -v

# 모든 branch 업로드
$ git push origin master develop 1.0.0

$ git push -u origin mast
Username for 'http://gitlab.35.209.207.26.nip.io': root
Password for 'http://root@gitlab.35.209.207.26.nip.io':
Enumerating objects: 12, done.
Counting objects: 100% (12/12), done.
Delta compression using up to 2 threads
Compressing objects: 100% (7/7), done.
Writing objects: 100% (12/12), 820 bytes | 410.00 KiB/s, done.
Total 12 (delta 3), reused 0 (delta 0), pack-reused 0
To http://gitlab.35.209.207.26.nip.io/cloud-branch/gitflow-test/edu01.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.


```







## 9) ICIS-TR 에서는

* Git 







## 10) 마무리

* Git Flow 는 Code 관리 모델이며  Branch를 어떻게 관리하겠다는 전략이자 약속
* Git Flow 를 기반으로 현장 상황에 맞도록 커스텀이 가능

* 그 조직에 맞는 특별한 브랜치 전략이 없다면 Git-flow를 준수하여 적용하는 것도 나쁘지 않은 선택임











# 2. gitlab 샘플 프로젝트 이해 및 분기

gitlab 샘플 프로젝트 이해 및 분기

- 실습
- git clone
- git commit push
- branch checkout





git 사용 3단계

1단계 : main 브렌치에서 커밋과 푸시만 하는 경우

2단계 : 브랜치와 병합을 사용할 수 있고, switch와 restore 를 이용해 롤백을 사용

3단계 : 둘 이상의 원격 저장소를 활용하여 소스 코드를 관리하고 협업





### git init

#### 디렉토리 생성

```sh
$ mkdir ~/temp/gittest
  cd ~/temp/gittest

```





untracked 상태인 파일을 모두 삭제

```sh

$ git init

  git config --global user.email "ssongmantop@gmail.com"
  git config --global user.name "ssongman"

$ git status
```







### git clean

신규 파일을 생성후 이를 원복하고자 할때  사용됨

untracked 상태인 파일을 모두 삭제

```sh
$ cd ~/song/gittest


# 신규 파일 생성
$ echo "some1" > some.txt
  mkdir somedir
  echo "some2" > somedir/some.txt


$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        somedir/

nothing added to commit but untracked files present (use "git add" to track)

$ git clean -f -d
$ git status
On branch master
nothing to commit, working tree clean

$ ll
total 16
drwxrwxr-x 3 ktdseduuser ktdseduuser 4096 Sep 17 14:23 ./
drwxrwxr-x 3 ktdseduuser ktdseduuser 4096 Sep 17 14:07 ../
drwxrwxr-x 8 ktdseduuser ktdseduuser 4096 Sep 17 14:23 .git/


```





### hard reset

git reset 명령은 공통적으로 커밋을 되돌릴 때 사용한다.

가장 많이 사용하는 명령은 `reset --hard` 이다.

```sh
$ cd ~/song/gittest

git switch master

git switch -c reset-test

$ git branch -l
  master
* reset-test

git log --oneline -n1



git log --oneline -n3
e096ab8 (HEAD -> reset-test) reset 테스트용 커밋
42b5bd8 update


# 이전 커밋 형상으로 reset
$ git reset --hard HEAD~
HEAD is now at 42b5bd8 update

# 원복 완료됨

$ git status
On branch reset-test
nothing to commit, working tree clean



# hard reset 의 복구
$ git reset --hard e096ab8
HEAD is now at e096ab8 reset 테스트용 커밋



# 
# [다시] 이전 커밋 형상으로 reset
$ git reset --hard 42b5bd8
HEAD is now at 42b5bd8 update


$ git log  --oneline -n2
42b5bd8 (HEAD -> reset-test) update

```



로컬 저장소의 커밋은 없어지지않았다.살진 것처럼 안보일 뿐이다. 





### git merge theirs/ous 옵션 사용



```sh
$ cd ~/song/gittest/our-project

$ git branch --show-current
reset-test



echo "main1" > main1.txt
echo "main2" > main2.txt

git add .
git commit -m "update init"



# feature1 생성후 전환
git checkout -b feature1


echo "conflict1" > main2.txt

git add .

git commit -m "충돌1"


git log --oneline --all --graph -n3
* 2817a75 (HEAD -> feature1) 충돌1
* 199ceee (main) update init
* 8b04559 update



git diff main

diff --git a/main2.txt b/main2.txt
index 2041184..1b9074b 100644
--- a/main2.txt
+++ b/main2.txt
@@ -1 +1 @@
-main2
+conflict1



## 병합시도시 출동 발생

(main) git merge feature1

Updating 199ceee..2817a75
Fast-forward
 main2.txt | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)

```





병합 대상 브렌치 남기기

```sh
git merge feature1 -X theirs


cat main2.txt
```



main 브렌치의 내용 남기기

```sh

git log --oneline -n2


# hard reset 으로 병합커밋 되돌리기
git reset --hard HEAD~



# ours 옵션으로 병합
git merge feature1 -X ours



cat main2.txt
```







# 3. [참고] 기타



## 1) git prompt 생성

리눅스에서 git prompt 를 보기 위해서는 아래와 같은 작업이 필요하다.

```sh

$ vi ~/.bachrc
...
parse_git_branch() {
     git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/(\1)/'
}
export PS1="\e[01;32m\u@\h \[\e[34m\]\w\[\e[91m\]\$(parse_git_branch)\[\e[00m\]$ "



$ source ~/.bashrc

```

