# Git Flow Branch 전략







# 1. Git Flow Branch 전략















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

untracked 상태인 파일을 모두 삭제

```sh

$ mkdir -p ~/song/gittest/our-project
  cd ~/song/gittest/our-project


$ git init

  git config --global user.email "ssongmantop@gmail.com"
  git config --global user.name "ssongman"

$ git status
```







### git clean

untracked 상태인 파일을 모두 삭제

```sh
$ cd ~/song/gittest/our-project


$ echo "some1" > some.txt
$ mkdir somedir
$ echo "some2" > somedir/some.txt
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
-rw-rw-r-- 1 ktdseduuser ktdseduuser    6 Sep 17 14:22 some.txt


```





### hard reset

git reset 명령은 공통적으로 커밋을 되돌릴 때 사용한다.

가장 많이 사용하는 명령은 `reset --hard` 이다.

```sh
$ cd ~/song/gittest/our-project

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

