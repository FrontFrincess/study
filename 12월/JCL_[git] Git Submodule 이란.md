# Git Submodule 이란?



회사에서 깃 클론을 받고 돌려보는데 계속 에러가 뜨는 것임!! 그래서 엥 왜지? 했더니 

git submodule update --init --remote --recursive 를 입력하면 해결된다고 했다. 

그 당시에는 그냥 아 그렇구나 하고 넘어갔는데 실제로 이 서브 모듈을 이용해야 할 일이 많아서 이게 정확하게 뭔지 알아보기로 했다. 

## (1) Submodule 이란?

> Git 저장소 안에 다른 Git 저장소를 디렉토리로 분리해서 넣는 것이 서브모듈이다. 다른 독립된 Git 저장소를 클론해서 Git 저장소 안에 포함할 수 있으며 각 저장소의 커밋은 독립적으로 관리한다.  

즉, 서브 모듈을 사용하면 하나의 Git 저장소에서 여러 개의 프로젝트를 관리할 수 있으며, 각 프로젝트는 별도의 저장소로 유지됩니다. 이를 통해 복잡한 프로젝트를 여러 저장소로 분할하여 개발, 관리, 배포를 용이하게 할 수 있습니다. 


예를 들어, 복수의 레포지토리에서 공통으로 사용하는 여러 상수나 로직 등 공통으로 사용되는 부분을 서브 모듈로 분리해서 관리가 가능하며, 민감한 정보는 프라이빗 레포지토리에 저장한 뒤 이 레포를 서브모듈로 등록하여 사용 할 수 있다. 


실제로 해보면서 서브 모듈이 무엇인지 알아보도록 하자! 

## (2) Submodule 실습

일단 git 저장소를 두 개를 만들어보겠다. 

Github에 parent-repo / sub-repo 라는 이름의 레포지토리를 생성하겠다.
![](https://velog.velcdn.com/images/chaedev3/post/0c6df39f-8fce-4152-90dd-3ac461ddf7cf/image.png)
이렇게 repo 2개를 생성해주고 이 레포들을 로컬 환경에 클론하겠다. 

![](https://velog.velcdn.com/images/chaedev3/post/8a1f1639-0a71-4f39-9b7c-02b613d70412/image.png)
두 레포를 클론을 받은 후에는 sub-repo 에 빈 파일을 하나 추가해서 푸시해보자 
```bash
touch test 
git add .
git commit -m "first commit"
git push 
```

이제 parent-repo의 lib 디렉토리에 sub-repo를 서브 모듈로 등록해보자
parent-repo의 디렉토리로 이동한 다음 아래 명령을 입력하면 서브 모듈을 등록할 수 있다.

```bash
git submodule add (sub_repo git 주소) lib 
```

이러면 parent-repo 디렉토리에 sub-repo가 클론되며, 서브모듈이 등록된다. 
![](https://velog.velcdn.com/images/chaedev3/post/7ae22dce-6087-4c4b-b77c-46405d6545d2/image.png)
이렇게 lib 이라는 디렉토리가 생성된걸 볼 수 있다. 
추가로 .gitmodules란 파일이 생성되었는데 이 파일에는 서브 디렉토리와 서브 모듈 저장소 URL 이 매핑된 설정 정보가 담겨져있다.
```bash
$ cat .gitmodules 
[submodule "lib"]
	path = lib
    url = https://github.com/chaedev3/sub_repo.git 
```
gitmodules 도 gitignore와 마찬가지로 버전관리 대상이다. 이 파일을 통해 프로젝트 참여자들이 이 프로젝트에 어떤 서브 모듈이 있는 지 확인할 수 있다. 


```bash
$git diff --cached lib 
diff --git a/lib b/lib
new file mode 160000
index 0000000..93e43bb
--- /dev/null
+++ b/lib
@@ -0,0 +1 @@
+Subproject commit 93e43bb2c0044e284ba33e90a0990c018682275e

```
git diff 명령을 사용하면 git 디렉토리에서 파일이 변경된 내용을 확인할 수 있다. --cached (or --staged) 옵션은 아직 커밋되지 않고, 스테이지에 올라간 파일 대상으로 변경 내용을 확인한다. 그리고 lib은 변경 사항을 확인할 대상이다. 




그런데 나는 lib 디렉토리에 서브모듈이 클론되고, test라는 파일을 생성했는데 git diff의 결과물에는 보이지 않는 것을 알 수 있다. 
git 은 이 lib 디렉토리를 서브모듈로 취급하기 때문에, lib 하위 파일의 변경사항을 추적하지 않는다. 대신 서브 모듈 디렉토리를 통째로 특별한 커밋으로 취급한다. 

마지막 줄에 있는 93e4.. 커밋 해시를 보고 서브 모듈 디렉토리로 이동해서 아래와 같은 명령어를 입력한다. 
```bash
$ git log
commit 93e43bb2c0044e284ba33e90a0990c018682275e (HEAD -> master, origin/master,
origin/HEAD)
Author: chaedev3 <chaedev3@gmail.com>
Date:   Mon Dec 18 15:52:34 2023 +0900

    first commit
 
```
git diff 명령을 사용했을때 출력된 커밋 해시와 동일한 것을 알 수 있다. 즉, parent-repo 는 서브 모듈의 특정 커밋을 가리키고 있는 것이다.! 


그러면 이제 서브 모듈을 변경해보고 그 변경 내용을 parent-repo에 반영해보자.
```bash
$ touch test2
$ git add .
$ git commit -m "second commit" 
$ git push 
```
sub-repo 에서 아래 명령을 이용해 커밋을 해보자

그럼 이제 parent-repo에 가서 pull을 진행해보자 
```bash
$ git pull  
Already up to date. 
```
/parent_repo 에서 git pull을 진행하니 나는 sub-repo에서 바꾸었는데 pull을 해오지 않는다. lib으로 가야지만 pull을 받아올 수 있는데 그 이유는 lib은 parent-repo와 독립적인 저장소이기 때문이다. 

lib 디렉토리로 이동한 다음 다시 pull을 해보자 
```bash
$ cd lib 
$ git pull 
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 2 (delta 0), reused 2 (delta 0), pack-reused 0
Unpacking objects: 100% (2/2), 241 bytes | 40.00 KiB/s, done.
From https://github.com/chaedev3/sub_repo
   93e43bb..1a3ccf3  master     -> origin/master
Updating 93e43bb..1a3ccf3
Fast-forward
 test2 | 0
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 test2
```
정상적으로 pull 되고 , test2라는 파일도 생성이 되었다. 다시 git diff로 확인을 해보면 

```bash
$ git diff
diff --git a/lib b/lib
index 93e43bb..1a3ccf3 160000
--- a/lib
+++ b/lib
@@ -1 +1 @@
-Subproject commit 93e43bb2c0044e284ba33e90a0990c018682275e
+Subproject commit 1a3ccf3dcd2af354766dcb9c872f9aead71af97e
```
커밋이 변경된 것을 알 수 있다. 

## (3) 그 외 

더 편하게 서브 모듈 최신 커밋 가져오기
```bash
$ git submodule update --remote 
```
이렇게 하면 pull 명령을 통해 서브 모듈의 최신 버전을 안가져와도 가능하다..! 저 명령어로 가능 


또한 submodule 한 저장소를 clone 하는 경우, 서브 모듈 디렉토리는 빈 디렉토리가 된다. 그렇기 때문에 submodule init과 함께 update 수행이 필요함
이럴 때는 git submodule update --init --recursive해주면 된다..!! 


### 참고
[https://git-scm.com/book/ko/v2/Git-도구-서브모듈](https://git-scm.com/book/ko/v2/Git-%EB%8F%84%EA%B5%AC-%EC%84%9C%EB%B8%8C%EB%AA%A8%EB%93%88) 
https://hudi.blog/git-submodule/