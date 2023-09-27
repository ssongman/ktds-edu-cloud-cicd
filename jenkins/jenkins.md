### 1. Jenkins 란?

![img](asset/jenkins/img.jpg)



모든 언어의 조합과 소스 코드 레포지토리에 대한 지속적인 통합과 지속적 전달 그리고 지속적 배포 환경을 구축하기 위한 도구다. 

빌드, 테스트, 배포 프로세스를 자동화하여 소프트웨어 품질과 개발 생산성을 높일 수 있다.

| 웹사이트        | jenkins-ci.org |
| --------------- | -------------- |
| 발표일          | 2011년 2월 2일 |
| 프로그래밍 언어 | Java           |
| 최근 버전       | 2.23.3         |
| 운영체제        | 크로스 플랫폼  |
| 종류            | 지속적 통합    |
| 라이선스        | MIT            |



**편리한 설정**

웹 기반의 콘솔로 다양한 인증 기반과 결합이 가능하며 권한 관리 기능을 통해 안전한 빌드/배포 환경을 구축할 수 있다. 수많은 플러그인을 사용하여 자동화 할 수 있어 반복되는 작업을 줄일 수 있다. 빌드/배포의 결과에 대해 통지 받을 수 있는 설정이 간편하고 다양한 채널을 통해 빠르게 피드백을 받을 수 있다.

 

**안정적인 빌드/배포 환경**

소스 버전 관리 툴과 연동하여 코드 변경을 감지하고, 자동화 테스트를 포함한 빌드를 수행하여 소프트웨어 품질을 향상시킬 수 있다. 자동화 테스트에는 코딩 표준 준수 여부 체크, 유닛 테스트, 통합 테스트 등을 설정할 수 있고 테스트 결과에 대한 피드백을 받아 잠재적인 오류를 사전에 예방할 수 있다. 빌드 결과물을 지속적으로 배포하도록 설정하여 개발 프로세스 전체를 자동화할 수 있다.



 **다양한 활용 및 손쉬운 확장**

Jenkins는 많이 사용 되고 있는 오픈 소스 소프트웨어로 문서화가 잘 되어 있다. 빌드/배포 이외에도 스케쥴링을 이용한 배치 작업에도 활용되는 등 다양한 적용 사례들을 참고할 수 있다. 플러그 인을 직접 개발하여 기능을 확장하는 것도 가능하다. (https://plugins.jenkins.io/)



---



#### 1.1 젠킨스의 역할 및 이점

개발중인 프로젝트에서 커밋은 매우 빈번히 일어나기 때문에 커밋 횟수만큼 빌드를 실행하는 것이 아니라 작업이 큐잉되어 자신이 실행될 차례를 기다립니다

코드의 변경과 함께 이뤄지는 이 같은 자동화된 빌드와 테스트 작업들은 다음과 같은 이점들을 가져다 줍니다.

- 프로젝트 표준 컴파일 환경에서의 컴파일 오류 검출
- 자동화 테스트 수행
- 정적 코드 분석에 의한 코딩 규약 준수여부 체크

이 외에도 젠킨스는 1,800여가지가 넘는 플러그인을 온라인으로 간단히 인스톨 할 수 있는 기능을 제공하고 있으며 쉘, 파이썬과 같은 스크립트를 이용해 손쉽게 자신에게 필요한 기능을 추가 할 수도 있다.



##### 각종 배치 작업의 간략화

프로젝트 기간 중에 개발자들은 순수한 개발 작업 이외에 DB셋업이나 환경설정, Deploy작업과 같은 단순 작업에 시간과 노력을 들이는 경우가 빈번하다. 데이터베이스의 구축, 어플리케이션 서버로의 Deploy, 라이브러리 릴리즈와 같이 이전에 CLI로 실행되던 작업들이 젠킨스 덕분에 웹 인터페이스로 손쉽게 가능해졌다.

##### Build 자동화의 확립

빌드 툴의 경우 Java는 maven과 gradle이 자리잡고 있으며, 이미 빌드 관리 툴을 이용해 프로젝트를 진행하고 있다면 젠킨스를 사용하지 않을 이유가 하나도 없다. 젠킨스와 연동하여 빌드 자동화를 통해 프로젝트 진행의 효율성을 높일 수 있다.

##### 자동화 테스트

자동화 테스트는 젠킨스를 사용해야 하는 가장 큰 이유 중 하나이며, 사실상 자동화 테스트가 포함되지 않은 빌드는 CI자체가 불가능하다고 봐도 무방하다. 젠킨스는 Subversion이나 Git과 같은 버전관리시스템과 연동하여 코드 변경을 감지하고 자동화 테스트를 수행하기 때문에 만약 개인이 미처 실시하지 못한 테스트가 있다 하여도 든든한 안전망이 되어준다. 제대로 테스트를 거치지 않은 코드를 커밋하게 되면 화난 젠킨스를 만나게 된다.

##### 코드 표준 준수여부 검사

자동화 테스트와 마찬가지로 개인이 미처 실시하지 못한 코드 표준 준수 여부의 검사나 정적 분석을 통한 코드 품질 검사를 빌드 내부에서 수행함으로써 기술적 부채의 감소에도 크게 기여한다.

##### 빌드 파이프라인 구성

2개 이상의 모듈로 구성되는 레이어드 아키텍처가 적용 된 프로젝트에는 그에 따는 빌드 파이프라인 구성이 필요하다. 예를 들면, `도메인 -> 서비스 -> UI`와 같이 각 레이어의 참조 관계에 따라 순차적으로 빌드를 진행하지 않으면 안된다. 젠킨스에서는 이러한 빌드 파이프라인의 구성을 간단히 할 수 있으며, 스크립트를 통해서 매우 복잡한 제어까지도 가능하다.



---



#### 1.2 젠킨스 CI 프로세스

![image-20230918171236605](asset/jenkins/image-20230918171236605.png)

a. 특정 마이크로서비스의 변경사항이 Source repo에 commit 된다.

b. Jenkins의 Build Job이 해당 변경사항을 감지 또는 수동으로 Job을 실행하여 Docker Image 빌드를 수행한다.

c. 빌드의 결과물은 Nexus Image Repository에 Push된다. 



---



#### 1.3 젠킨스 with VM VS Kubernetes 

**VM Jenkins**

기존 VM으로 운영되던 Jenkins는 master가 있고 slave(vm n대 설정)를 연결시키면 slave가 job을 master로 부터 받아서 수행했엇다.

하지만 VM으로 운영되는 Jenkins(master-slave) 구조는 아래와 같은 단점이 있다.

- Master 는 Slave 를 관리하기위해 지속적인 커넥션 및 설정 관리필요
- Job에 비해서 slave가 많으면 비효율적임(slave가 놀고 있음)
- Job에 비해서 slave가 적으면 비효율적임(slave가 부족해서 기다리는 job이 많아짐)
- 즉, Job이 늘어나고 줄어듦에 따라 Jenkins slave를 늘리거나 줄여야하는(사람이 직접 설치해야하는) 단점

![image-20230918180409621](asset/jenkins/image-20230918180409621.png)



**Kubernetes Jenkins**

Cluster 환경에서 Kubernetes Plugin 적용 후 아래와 같은 이점이 생깁니다.

- Jenkins master는 단순히 job을 정의하고 저장하는 역할
  **- 관리가 한결 편해진다.**
- Jenkins slave는 쿠버네티스 클러스터에서 Pod으로 동적 생성/삭제됨
  **- 확장 용이성 증가로 리소스의 효율화**
- 각 Job에 따라 필요한 리소스를 각각 정의하여 관리가능
  **- 각 Job에 따라 필요한 리소스(maven, gradle, docker 등)를 직접 pipeline에 정의하여 사용
  \- master가 plugin들을 관리할 필요가 없음!**

##### 젠킨스(with kubernetes plugin) 파이프라인 생명주기

![img](asset/jenkins/991EA9445B2F372827)

1. Jenkins(with kubernetes plugin) master에게 Jenkins job 수행요청
2. Jenkins master는 kubernetes에게 slave를 생성하도록 선언
3. 새로 생성된 Jenkins slave pod은 job을 수행(빌드, 배포 등)
4. job이 완료되면 해당 Jenkins slave pod을 삭제



---



### 2. 사전준비

https://www.docker.com/get-started/

#### 2.1 Kubernetes Resourece 준비

**serviceAccount**: 쿠버네티스 클러스터 내에서 실행되는 팟(Pod)이 API 서버와 상호 작용할 수 있도록 **권한을 부여하는 데 사용되는 자격증명**입니다. 
서비스 어카운트는 특정 네임스페이스(namespace)에 속하며, 자동으로 생성되거나 사용자가 직접 생성할 수 있습니다. 

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: jenkins-admin
  namespace: user02
```



**ClusterRole**: 롤(Role)은 특정 api나 리소스에 대한 권한들을 명시해둔 규칙들의 집합입니다. 롤에는 일반 롤(Role)과 클러스터롤(ClusterRole) 2가지 종류가 있습니다. 롤은 그 롤이 속한 네임스페이스에 한곳에만 적용됩니다. 클러스터롤은 특정 네임스페이스에 대한 권한이 아닌 **클러스터 전체에 대한 권한을 관리**합니다.

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: jenkins-admin
rules:
  - apiGroups: ["*"]
    resources: ["*"]
    verbs: ["*"]
```



**ClusterRoleBinding**: 클러스터롤바인딩은 클러스터롤과 사용자를 묶어(binding)주는 역할을 합니다. 
즉 어떤 사용자가 무슨 롤을 사용하는지를 지정해 줍니다. 또한 권한이 롤과 클러스터롤로 구분되어 있는 것처럼 바인딩로 롤바인딩과 클러스터롤바인딩으로 구분됩니다

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: jenkins-admin
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: jenkins-admin
subjects:
- kind: ServiceAccount
  name: jenkins-admin
  namespace: user02
```



---

#### 2.2 Gitlab

##### 2.2.1 GitLab Token 

http://gitlab.ssongman.duckdns.org/

![image-20230919090743189](asset/jenkins/image-20230919090743189.png)

![image-20230919090855977](asset/jenkins/image-20230919090855977.png)

![image-20230919091107663](asset/jenkins/image-20230919091107663.png)



![image-20230919091158245](asset/jenkins/image-20230919091158245.png)

---



**GitHub 사용시**

https://github.com/settings/tokens or github -> settings -> developer settings -> Personal access tokens

**Generate new Token**

![image-20230916201904004](asset/jenkins/image-20230916201904004.png)

**Select Scopes**

![image-20230916204106190](asset/jenkins/image-20230916204106190.png)



**Token 저장**

![image-20230918151331800](asset/jenkins/image-20230918151331800.png)



---



##### 2.2.2 프로젝트 생성



**New Project**

![image-20230923002419919](asset/jenkins/zzxcvasdasdasd.png)



**Import Project**

![image-20230923002522058](asset/jenkins/image-20230923002522058.png)

**Import Project 정보입력**(http://gitlab.ssongman.duckdns.org/cjs/base-project.git)

![image-20230923002120221](asset/jenkins/asdasdfzxcv.png)

**Project Clone**

```bash
#base-project clone
$ git clone http://user02:${GIT_TOKEN}@gitlab.ssongman.duckdns.org/cjs/base-project.git

Credential : ${GIT_TOKEN} 입력

```

**user02, ${GIT_TOKEN} 일괄 변경**



---





#### 2.3 Dockerfile 작성 및 빌드 테스트(Local)

**Dockerfile 이란?**

docker에서 이용하는 이미지를 기반으로 새로운 이미지를 스크립트 파일을 통해 내가 설정한 나만의 이미지를 생성할 수 있는 일종의 이미지 설정파일입니다.

dockerfile을 이용하면 컨테이너 생성 시 기존 이미지로 생성하였을 경우 추가로 설정해줘야 할 것들을 미리 설정 및 사전 준비가 가능하므로 잘 이용할 수 있다면 무궁무진한 형태의 이미지를 좀 더 간편하고 손쉽게 만들어 낼 수 있습니다.

**Dockerfile 명령어 **

```dockerfile
FROM
```

docker 이미지 생성에 있어서 기초가 되는 베이스 이미지를 지정합니다. Dockerfile 작성에 있어서 가장 기초가 되는 부분이며 이미지를 로컬에 가지고 있을 경우 그냥 사용하지만, 없다면 도커 허브에서 제공하는 이미지를 불러오게 되어 있습니다.



```dockerfile
MAINTAINER
```

해당 이미지의 생성, 유지, 보수, 등의 즉 관리자를 뜻하는 란입니다. (실질적으로는 효과는 없음)

 

```dockerfile
RUN
```

쉘 명령어를 사용 할 수 있도록 해줍니다. &나 |, \ 등을 통해 줄여주실 수 있다면, 줄여주시는 것이 좋습니다. 주로 packgage 설치나 기본 설정에 사용합니다.

 

```dockerfile
WORKDIR 
```

쉘의 cd 명령문처럼 컨테이너 상에서 작업 디텍토리로 전환을 위해서 사용됩니다. WORKDIR 명령문으로 작업 디렉터리를 전환하면 그 이후에 등장하는 모든 RUN, CMD, ENTRYPOINT, COPY, ADD 명령문은 해당 디렉터리를 기준으로 실행됩니다.



```dockerfile
ARG
```

`docker build` 커맨드로 이미지를 빌드 시, `--build-arg` 옵션을 통해 넘길 수 있는 인자를 정의하기 위해 사용합니다.



```dockerfile
VOLUME
```

이 이미지로 만든 컨테이너와 호스트 간의 연결이 가능한 볼륨을 지정해줍니다.

이는 이미지 생성 후 docker run 명령어 사용 시 -v 옵션과 함께 <호스트 볼륨>:<컨테이너 볼륨>의 모양으로 설정하게 되며, 설정이 없을 경우, 이미지에서 제공하는 파일들이 들어가있게 됩니다.

 

```dockerfile
EXPOSE
```

컨테이너와 호스트 간 연결이 가능한 포트를 지정해주게 됩니다.



 ```dockerfile
ADD 
COPY
 ```

ADD와 COPY는 이미지로 파일을 불러오는 기능을 합니다.

차이점이라면 COPY의 경우 실제 로컬에서만 파일을 불러 올 수 있으며, 파일 자체를 가져오게 됩니다. ADD의 경우 url을 사용하여 외부에서도 파일을 가져 올 수 있으며, tar로 압축 된 파일을 풀어서 가져오게 됩니다. 다만 인터넷 url을 통해 가져온 파일의 경우는 tar의 경우에도 원본으로 가져옵니다.




```dockerfile
ENTRYPOINT
CMD
```

docker run 명령어 실행, 멈춰있던 컨테이너가 다시 시작될 경우 등에 쉘 명령어 및 스크립트를 변수를 받아 사용할 수 있습니다.

이 두 명령어의 경우 용도 및 사용법이 매우 비슷합니다.

 

```dockerfile
ENTRYPOINT ["/usr/sbin/apachectl", "-D", "FOREGROUND"]
#/usr/sbin/apachectl 이라는 쉘 스크립트를 -D, FOREGROUND라고 하는 인자 값들을 받아 실행시키겠다는 의미입니다.

ENTRYPOINT apachectl -D FOREGOUND
#이렇게 입력을 해줘도 되지만 위와 같이 입력을 해주는 것이 좀 더 정확하게 인식을 한다고 합니다.
#왜냐하면 이렇게 입력하는 것은 기본을 /bin/sh -c 명령어로 호출하여 실행하기 때문입니다.

#RUN 명령어는 위에서 언급했듯 기초 세팅에 필요한 부분에서 이용하는 것이 좋습니다
#ENTRYPOINT의 경우 dockerfile 내에서 단 1개만 작동 합니다.

#CMD의 경우 여러개를 선언하여도 override가 가능합니다. ENTRYPOINT와 CMD가 같이 쓰이기도 하는데,
ENTRYPOINT ["/bin/echo","Hello"]
CMD ["world"] 
#라고 작성할 경우 결과물로 컨테이너 실행 시 Hello world를 볼 수 있습니다.
```



##### 2.3.1 Maven(Java)

Sample Project

```bash
$ cd sample/hello-world-spring/demo
```



Dockerfile 작성

```dockerfile
FROM maven:3.8.4-openjdk-17
WORKDIR /app
COPY . .
RUN ["mvn", "clean", "package", "-DfinalName=app"]
RUN ["cp", "target/app.jar", "app.jar"]
ENTRYPOINT ["java", "-jar", "app.jar"]
```



Image Build & Push

```bash
$ docker build -t spring-test:1.0.0 .
[+] Building 21.6s (7/7) FINISHED
...
 => => exporting layers                                                                                            0.1s
 => => writing image sha256:b58c2fba32a9df3028874a4e51f119a68f18d5d23254412cc40c3bf08b378770                       0.0s
 => => naming to docker.io/library/spring-test:1.0.0                                                               0.0s
 
$ docker images
REPOSITORY                                                     TAG              IMAGE ID       CREATED              SIZE
spring-test                                                    1.0.0            b58c2fba32a9   About a minute ago   425MB

$ docker run -d -p 8080:8080 spring-test:1.0.0
e66ecbfeda0c7c37211bd3c18755b56c8344f88a5d265d966b1aec11ab7877c0


$ curl localhost:8080
Hello Spring World

$ docker tag spring-test:1.0.0 nexus-repo.ssongman.duckdns.org/user02/spring-test:1.0.0
$ docker push nexus-repo.ssongman.duckdns.org/user02/spring-test:1.0.0

```



Image Push 확인

```
http://nexus.ssongman.duckdns.org
```



##### 2.3.2 Npm(node.js) 

Sample Project

```bash
$ cd sample/hello-world-express
```



Dockerfile 작성

```dockerfile
FROM node:16
WORKDIR /usr/src/app

COPY package*.json ./
RUN npm install

COPY . .
ENTRYPOINT ["node", "/app.js"]
```



Image Build & Push

```bash
$ docker build -t express-test:1.0.0 .
[+] Building 10.1s (4/6)
 => [internal] load build definition from Dockerfile 0.0s
 => => transferring dockerfile: 191B 0.0s
 => [internal] load .dockerignore 0.0s
...
 => [2/2] COPY /server.js /server.js                                                                                        0.9s
 => exporting to image                                                                                                      0.0s
 => => exporting layers                                                                                                     0.0s
 => => writing image sha256:e63790a33616a9b048e1b52e64f1bd7e6f03ea573f55357769ec3e6389a2777c                                0.0s
 => => naming to docker.io/library/express-test:1.0.0                                                                       0.0s

$ docker images
REPOSITORY                                                 TAG              IMAGE ID       CREATED         SIZE
express-test                                               1.0.0            e63790a33616   1 minutes ago   909MB

$ docker run -d -p 3000:3000 express-test:1.0.0
7cc6e3a27c94cfd96a315e9d492e9df636c146dcaefd590841a9df83b398524b

$ docker ps 
CONTAINER ID   IMAGE                COMMAND                  CREATED             STATUS             PORTS                    NAMES
275b1fc74dcb   express-test:1.0.0   "docker-entrypoint.s…"   2 seconds ago       Up 1 second        0.0.0.0:3000->3000/tcp   zen_khorana

$ curl localhost:3000
Hello Express World

$ docker tag express-test:1.0.0 nexus-repo.ssongman.duckdns.org/user02/express-test:1.0.0

$ docker push nexus-repo.ssongman.duckdns.org/user02/express-test:1.0.0
1.0.0: digest: sha256:b93ce4d64616e232916dbbf8938921b42a72de64652069e2b3ff89d26e54a219 size: 2836

```

Image Push 확인

```
http://nexus.ssongman.duckdns.org
```



##### 2.3.3 Flask(python) 

Sample Project

```bash
$ cd sample/hello-world-flask
```



Dockerfile 작성

```dockerfile
FROM python:3.9.6-slim-buster
WORKDIR /app


COPY requirements.txt requirements.txt
# Pip 패키지 설치
RUN pip3 install -r requirements.txt

# 필요한 파일을 복사
COPY . .
ENTRYPOINT ["python3", "app.py"]
```



Image Build & Push

```bash
$ docker build -t flask-test:1.0.0 .
[+] Building 5.0s (9/9) FINISHED
 => [internal] load build definition from Dockerfile                           0.0s
 => => transferring dockerfile: 179B                                           0.0s
 => [internal] load .dockerignore                                              0.0s
 => => transferring context: 2B                                                0.0s
 => [internal] load metadata for docker.io/library/python:3.8                  0.8s
 => [internal] load build context                                              0.0s
 => => transferring context: 240B                                              0.0s
 => CACHED [1/4] FROM docker.io/library/python:3.8@sha256:9c7e7                0.0s
 => [2/4] COPY . /app/server                                                   0.0s
 => [3/4] WORKDIR /app/server                                                  0.0s
 => [4/4] RUN pip install -r requirements.txt                                  3.7s
 => exporting to image                                                         0.2s
 => => exporting layers                                                        0.2s
 => => writing image sha256:b84ab2d27c54d652a21580f8eb40fdec71d                0.0s
 => => naming to docker.io/library/flask-test:1.0.0                            0.0s

$ docker images
REPOSITORY                                                 TAG              IMAGE ID       CREATED              SIZE
flask-test                                                 1.0.0            270dcda1b444   About a minute ago   68.4MB

$ docker run -d  --name flask-test -p 8082:8082 flask-test:1.0.0 
$ docker ps
CONTAINER ID   IMAGE                COMMAND             CREATED          STATUS          PORTS                    NAMES
b0d3cac56c35   flask-test:1.0.0     "python3 app.py"    3 seconds ago    Up 2 seconds    0.0.0.0:8082->8082/tcp   flask-test

$ curl localhost:8082
Hello Flask World

$ docker tag flask-test:1.0.0 nexus-repo.ssongman.duckdns.org/user02/flask-test:1.0.0
$ docker push nexus-repo.ssongman.duckdns.org/user02/flask-test:1.0.0
The push refers to repository [nexus-repo.ssongman.duckdns.org/user02/flask-test]
22e8ae1c18ff: Pushed
865a5ae0c9f4: Pushed
96eb9fd00fa3: Pushed
8a93afd5a022: Pushed
08928985481f: Pushed
7acf52b2a13c: Pushed
ce0f4c80e9b7: Pushed
9ad60c84bfbe: Pushed
4693057ce236: Pushed
1.0.0: digest: sha256:22da2f85ca92f5ddb0f74845bcad1e4b9d0e32f81a03b67998f8a0b2aabd20e5 size: 2200

```

Image Push 확인

```
http://nexus.ssongman.duckdns.org
```



---



#### 2.4 베이스 이미지[Build-tool] 업로드(Local)

kustomize 다운로드 URL : https://github.com/kubernetes-sigs/kustomize

![image-20230916011132592](asset/jenkins/image-20230916011132592.png)

압축 해제 후 Dockerfile 과 동일한 경로에 위치.

![image-20230916011301955](asset/jenkins/image-20230916011301955.png)

**podman build-tool**

```bash
$ cd sample/build-tool
$ cat Dockerfile
```



Dockerfile 작성

```dockerfile
#build 및 Image Push 를 위한 이미지 베이스 이미지
FROM mattermost/podman:1.8.0 
COPY ./kustomize /usr/local/bin/kustomize
```



Image Build & Push

```bash
$ docker build -t nexus-repo.ssongman.duckdns.org/user02/build-tool:1.0.0 .
[+] Building 26.4s (7/7) FINISHED
 => [internal] load build definition from Dockerfile                                                      0.0s
 => => transferring dockerfile: 109B                                                                      0.0s
 ...
 => [2/2] COPY ./kustomize /usr/local/bin/kustomize                                                       1.3s
 => exporting to image                                                                                    0.1s
 => => exporting layers                                                                                   0.1s
 => => writing image sha256:ce60ae205666db7a58314bf584977a5b5f4aa9d762edfccb76561bd1c5b63c24              0.0s
 => => naming to nexus-repo.ssongman.duckdns.org/build-tool:1.0.0                             0.0s

$ docker images
REPOSITORY                                                      TAG              IMAGE ID       CREATED              SIZE
nexus-repo.ssongman.duckdns.org/build-tool          1.0.0            ce60ae205666   About a minute ago   484MB

$ docker run -d nexus-repo.ssongman.duckdns.org/user02/build-tool:1.0.0 sleep 365d

$ docker ps
$ docker exec -it ${CONTAINER_ID} bash
$ podman version
Version:            1.8.0
RemoteAPI Version:  1
Go Version:         go1.11.6
OS/Arch:            linux/amd64
$ exit

$ docker push nexus-repo.ssongman.duckdns.org/user02/build-tool:1.0.0
The push refers to repository [nexus-repo.ssongman.duckdns.org/build-tool]
af60d788c1d5: Layer already exists
c4cfb19af9c8: Layer already exists
343ddf70e345: Layer already exists
89958da00402: Layer already exists
fb1696eb9af0: Layer already exists
dc3124d29e2e: Layer already exists
8439f45706c6: Layer already exists
1c76bd0dc325: Layer already exists
1.0.0: digest: sha256:3c400dd652e4f30a095745eda4812d722138a85b54808305ec108fbbf7b42d49 size: 1993

```

**maven-build-tool **

Dockerfile 작성

```dockerfile
FROM maven:3.8.4-openjdk-17
COPY ./kustomize /usr/local/bin/kustomize
```

Image Build & Push

```bash
$ docker build -t nexus-repo.ssongman.duckdns.org/user02/maven-build-tool:1.0.0 -f .\Dockerfile_maven .
[+] Building 3.0s (7/7) FINISHED
 => [internal] load build definition from Dockerfile_maven                             0.0s
 => => transferring dockerfile: 257B                                                   0.0s
 => [internal] load .dockerignore                                                      0.0s
 => => transferring context: 2B                                                        0.0s
 => [internal] load metadata for docker.io/library/maven:3.8.4-openjdk-17              2.9s
 => [internal] load build context                                                      0.0s
 => => transferring context: 33B                                                       0.0s
 => [1/2] FROM docker.io/library/maven:3.8.4-openjdk-17@sha256:7106ce47b5c226ee21bac38 0.0s
 => CACHED [2/2] COPY ./kustomize /usr/local/bin/kustomize                             0.0s
 => exporting to image                                                                 0.0s
 => => exporting layers                                                                0.0s
 => => writing image sha256:fb5cd9a2a8a34454fbf52e816e6dce0d24ac0b202cf30413542b513bc0 0.0s
 => => naming to nexus-repo.ssongman.duckdns.org/maven-build-tool:1.0.0    0.0s

$ docker images
REPOSITORY                                                      TAG       IMAGE ID       CREATED            SIZE
nexus-repo.ssongman.duckdns.org/user02/maven-build-tool    1.0.0     fb5cd9a2a8a3   About a minute ago 808MB

$ docker run -d nexus-repo.ssongman.duckdns.org/user02/maven-build-tool:1.0.0 sleep 365d

$ docker exec -it ${CONTAINER_ID} bash
$ mvn -version
Apache Maven 3.8.4 (9b656c72d54e5bacbed989b64718c159fe39b537)
Maven home: /usr/share/maven
Java version: 17.0.2, vendor: Oracle Corporation, runtime: /usr/java/openjdk-17
Default locale: en, platform encoding: UTF-8
OS name: "linux", version: "5.10.102.1-microsoft-standard-wsl2", arch: "amd64", family: "unix"
$ exit

$ docker push nexus-repo.ssongman.duckdns.org/user02/maven-build-tool:1.0.0
The push refers to repository [nexus-repo.ssongman.duckdns.org/maven-build-tool]
4d950bb416bc: Layer already exists
4ddad1bac86f: Layer already exists
c14a8489df05: Layer already exists
6907ac97c2f7: Layer already exists
0b4a4f5e5199: Layer already exists
4f97f905ddf2: Layer already exists
f7bb7102ce10: Layer already exists
43d0188e68eb: Layer already exists
1.0.0: digest: sha256:539cef2a15a53a96f4497522cfe092426d99eedd81acfa0b7a9e80dcb3035661 size: 2003

```

Image Push 확인

```
http://nexus.ssongman.duckdns.org
```



**npm-build-tool** 

Dockerfile 작성

```dockerfile
FROM node:16
COPY ./kustomize /usr/local/bin/kustomize
```

Image Build & Push

```bash
$ docker build -t nexus-repo.ssongman.duckdns.org/user02/npm-build-tool:1.0.0 -f .\Dockerfile_npm .
[+] Building 3.0s (7/7) FINISHED                                                      
 => [internal] load build definition from Dockerfile_npm                              0.0s
 => => transferring dockerfile: 206B                                                  0.0s
 => [internal] load .dockerignore                                                     0.0s
 => => transferring context: 2B                                                       0.0s
 => [internal] load metadata for docker.io/library/node:16                            2.7s
 => [internal] load build context                                                     0.0s
 => => transferring context: 33B                                                      0.0s
 => CACHED [1/2] FROM docker.io/library/node:16@sha256:f77a1aef2da8d83e45ec990f45df5  0.0s
 => [2/2] COPY ./kustomize /usr/local/bin/kustomize                                   0.1s
 => exporting to image                                                                0.1s
 => => exporting layers                                                               0.1s
 => => writing image sha256:57e6e02812f56ab3fdb029ead019e207a4462261cbf97c8680656e3d  0.0s
 => => naming to nexus-repo.ssongman.duckdns.org/npm-build-tool:1.0.0     0.0s


$ docker images
REPOSITORY                                                      TAG       IMAGE ID       CREATED            SIZE
nexus-repo.ssongman.duckdns.org/user02/npm-build-tool    1.0.0     fb5cd9a2a8a3   About a minute ago 924MB


$ docker run -d nexus-repo.ssongman.duckdns.org/user02/npm-build-tool:1.0.0 sleep 365d

$ docker exec -it ${CONTAINER_ID} bash
$ npm version
{
  npm: '8.19.4',
  node: '16.20.2',
  v8: '9.4.146.26-node.26',
  uv: '1.43.0',
  zlib: '1.2.11',
  brotli: '1.0.9',
  ares: '1.19.1',
  modules: '93',
  nghttp2: '1.47.0',
  napi: '8',
  llhttp: '6.0.11',
  openssl: '1.1.1v+quic',
  cldr: '41.0',
  icu: '71.1',
  tz: '2022f',
  unicode: '14.0',
  ngtcp2: '0.8.1',
  nghttp3: '0.7.0'
}
$ exit

$ docker push nexus-repo.ssongman.duckdns.org/user02/npm-build-tool:1.0.0
The push refers to repository [nexus-repo.ssongman.duckdns.org/npm-build-tool]
564b407be96a: Pushed
be322b479aee: Layer already exists
d41bcd3a037b: Layer already exists
fe0d845e767b: Layer already exists
f25ec1d93a58: Layer already exists
794ce8b1b516: Layer already exists
3220beed9b06: Layer already exists
684f82921421: Layer already exists
9af5f53e8f62: Layer already exists
1.0.0: digest: sha256:893ec32d1cb67cd44c2c1d25b8929f74defe50ecfd21bfddf0b92f2ef0cef6c7 size: 2215

```

Image Push 확인

```
http://nexus.ssongman.duckdns.org
```



**python-build-tool **

Dockerfile 작성

```dockerfile
FROM python:3.8
COPY ./kustomize /usr/local/bin/kustomize
```

Image Build & Push



```bash
$ docker build -t nexus-repo.ssongman.duckdns.org/user02/python-build-tool:1.0.0 -f .\Dockerfile_python .
[+] Building 7.5s (7/7) FINISHED
 => [internal] load build definition from Dockerfile_python                                       0.0s
 => => transferring dockerfile: 221B                                                              0.0s
 => [internal] load .dockerignore                                                                 0.0s
 => => transferring context: 2B                                                                   0.0s
 => [internal] load metadata for docker.io/library/python:latest                                  3.8s
 => [internal] load build context                                                                 0.0s
 => => transferring context: 33B                                                                  0.0s
 => [1/2] FROM docker.io/library/python:latest@sha256:cc7372fe4746ca323f18c6bd0d45dadf22d192756ab 3.2s
 => => resolve docker.io/library/python:latest@sha256:cc7372fe4746ca323f18c6bd0d45dadf22d192756ab 0.0s
 => => sha256:e2f1160974087f047a90d64ce50bd95d279c89309f32caeaa0b3503c253cab45 244B / 244B        0.5s
 => => sha256:8a164692c20c8f51986d25c16caa6bf03bde14e4b6e6a4c06b5437d5620cc96c 2.01kB / 2.01kB    0.0s
 => => sha256:22c957c35e37bdd688c2bdda50dc72612477d6c9c393802163b14d197a568bff 7.53kB / 7.53kB    0.0s
 => => sha256:0f546edb7ae0f7fecbac92a156849e2479dbf591ed0be9ac68e873da28c2a7a7 19.78MB / 19.78MB  1.7s
 => => sha256:cc7372fe4746ca323f18c6bd0d45dadf22d192756abc5f73e39f9c7f10cba5aa 2.14kB / 2.14kB    0.0s
 => => sha256:a0d3c67a6b6b0b67a9e4735c18ae78ca68e2252e0bed7e6fc3912dd5e0e8f042 3.11MB / 3.11MB    1.0s
 => => extracting sha256:0f546edb7ae0f7fecbac92a156849e2479dbf591ed0be9ac68e873da28c2a7a7         0.9s
 => => extracting sha256:e2f1160974087f047a90d64ce50bd95d279c89309f32caeaa0b3503c253cab45         0.0s
 => => extracting sha256:a0d3c67a6b6b0b67a9e4735c18ae78ca68e2252e0bed7e6fc3912dd5e0e8f042         0.3s
 => [2/2] COPY ./kustomize /usr/local/bin/kustomize                                               0.2s
 => exporting to image                                                                            0.1s
 => => exporting layers                                                                           0.1s
 => => writing image sha256:a9d3550eb858e1e56ec49eb4025b82cd368351919cc211d201546f8e21846370      0.0s
 => => naming to nexus-repo.ssongman.duckdns.org/python-build-tool:1.0.0              0.0s


$ docker images
REPOSITORY                                                      TAG       IMAGE ID       CREATED            SIZE
nexus-repo.ssongman.duckdns.org/user02/python-build-tool    1.0.0     fb5cd9a2a8a3   About a minute ago 1.02GB


$ docker run -d nexus-repo.ssongman.duckdns.org/user02/python-build-tool:1.0.0 sleep 365d

$ docker exec -it ${CONTAINER_ID} bash
$ python --version
Python 3.11.5
$ exit

$ docker push nexus-repo.ssongman.duckdns.org/user02/python-build-tool:1.0.0
The push refers to repository [nexus-repo.ssongman.duckdns.org/python-build-tool]
700a6240cd12: Pushed
49df279faf6c: Pushed
f2c0489561b5: Pushed
4831c7caec2d: Pushed
d2b487de5a01: Layer already exists
8fe5334a79c9: Layer already exists
acd413ce78f8: Layer already exists
1a26fac01f32: Layer already exists
b8544860ba0b: Layer already exists
1.0.0: digest: sha256:d61bf8efa50e5a2c0ca2d78486433b2aa1dc33f75a4bf31657b0465b8e9a5a97 size: 2218

```

Image Push 확인

```
http://nexus.ssongman.duckdns.org
```



---



### 3. Jenkins Helm 설치(Cluster)

#### 3.1 Helm Chart 준비

```bash
#helm 준비 
$ helm repo add jenkinsci https://charts.jenkins.io
"jenkins" has been added to your repositories

#helm repository 갱신
$ helm repo update 
Hang tight while we grab the latest from your chart repositories...
...Successfully got an update from the "gin" chart repository
...Successfully got an update from the "rhcharts" chart repository
...Successfully got an update from the "istio" chart repository
...Successfully got an update from the "hashicorp" chart repository
...Successfully got an update from the "atlassian-data-center" chart repository
...Successfully got an update from the "jenkins" chart repository
...Successfully got an update from the "prometheus-community" chart repository
...Successfully got an update from the "bitnami" chart repository
Update Complete. ⎈Happy Helming!⎈

#추가된 helm repository 내 jenkins 검색(23.09.01기준 버전 목록 )
$ helm search repo jenkins
NAME                    CHART VERSION   APP VERSION     DESCRIPTION
bitnami/jenkins         12.3.2          2.414.1         Jenkins is an open source Continuous Integratio...
jenkins/jenkins         4.6.4           2.414.1         Jenkins - Build great things at any scale! The ...
jenkinsci/jenkins       4.6.4           2.414.1         Jenkins - Build great things at any scale! The ...

#helm chart download
$ helm pull jenkinsci/jenkins --version 4.6.4

$ ls
jenkins-4.6.4.tgz

#압축해제
$ tar -zxvf jenkins-4.6.4.tgz 
jenkins/Chart.yaml
jenkins/values.yaml
jenkins/templates/NOTES.txt
jenkins/templates/_helpers.tpl
jenkins/templates/config-init-scripts.yaml

...

jenkins/templates/tests/test-config.yaml
jenkins/CHANGELOG.md
jenkins/README.md
jenkins/VALUES_SUMMARY.md

#TimeZone 설정
$ vi values.yaml

controller:

...

  javaOpts: "-Duser.timezone=Asia/Seoul"

!wq
```

#### 3.1 Helm Install 

```bash
$ cd jenkins

# values.yaml 확인
$ vi values.yaml

#helm chart install 
$ helm -n user02 install jenkins jenkinsci/jenkins --version=4.6.4 -f values.yaml \
--set controller.ingress.enabled=true \
--set controller.adminPassword=new1234! \
--set serviceAccount.create=false \
--set serviceAccount.name=jenkins-admin \
--set persistence.enabled=false \
--set agent.resources.requests.cpu=1024m \
--set agent.resources.requests.memory=1024Mi \
--set agent.resources.limits.cpu=1024m \
--set agent.resources.limits.memory=1024Mi


--set controller.ingress.hostName=jenkins.user02.cloud.35.209.207.26.nip.io \


    image: "jenkins/inbound-agent:3142.vcfca_0cd92128-1"
    name: "jnlp"
    resources:
      requests:
        memory: "256Mi"
        cpu: "100m"
        
        

#helm chart delete
$ helm -n user02 delete jenkins

```



---



### 4. jenkins 설정

Jenkins 접속(http://jenkins.user02.cloud.35.209.207.26.nip.io)

![image-20230918211841802](asset/jenkins/image-20230918211841802.png)



#### 4.1 플러그인 업데이트 

**Jenkins 관리로 이동**

![](asset/jenkins/image-20230916200438207.png)

**Plugins 관리 이동**

![image-20230916201049526](asset/jenkins/image-20230916201049526.png)



**Plugin Update**

전체선택 후 Update 클릭

![image-20230918212051456](asset/jenkins/image-20230918212051456.png)

**Plugin install**

![image-20230923010016083](asset/jenkins/image-20230923010016083.png)





#### 4.2 Credentials 설정

**Jenkins관리 -> System 이동**

![image-20230916205042219](asset/jenkins/image-20230916205042219.png)



**global-> Add Credeitnals 이동**

![image-20230916205235031](asset/jenkins/image-20230916205235031.png)

**정보입력**

![image-20230923010524655](asset/jenkins/image-20230923010524655.png)





---





#### 4.3 Global Properties 설정

Jenkins 관리 -> System 이동

![image-20230916204620570](asset/jenkins/image-20230916204620570.png)

**Global Properties 설정**(스크롤하여 Global Properties 확인)

![image-20230918143337804](asset/jenkins/image-20230918143337804.png)



---



### 5. Jenkins 파이프라인 유형별 템플릿

**새로운Item 이동**

![image-20230918182630127](asset/jenkins/image-20230918182630127.png)

**Pipeline Name 입력 -> Pipeline 선택 -> OK 클릭**

![image-20230918182822928](asset/jenkins/image-20230918182822928.png)

**Jenkins Job 생성**(1) - Pipeline Script from SCM

![image-20230922175504198](asset/jenkins/image-20230922175504198.png)



**Jenkins Job 생성**(2) - Pipeline

![image-20230918183020333](asset/jenkins/image-20230918183020333.png)



#### 5.1 maven 템플릿

```groovy
def label = "hello-${UUID.randomUUID().toString()}"
podTemplate(label: label,
	containers: [
        containerTemplate(name: 'maven', image: 'maven:alpine', ttyEnabled: true, command: 'cat')
  ]) {

    node(label) {
        stage('Maven') {
            container('maven') {
                    sh 'mvn -version'
            }
        }
    }
}
```



#### 5.2 node 템플릿

```groovy
def label = "hello-${UUID.randomUUID().toString()}"
podTemplate(label: label,
	containers: [
        containerTemplate(name: 'node', image: 'node:18.17.1-alpine3.18', ttyEnabled: true, command: 'cat')
  ]) {

    node(label) {
        stage('node') {
            container('node') {
                    sh 'node --version'
            }
        }
    }
}
```



#### 5.3 python 템플릿

```groovy
def label = "hello-${UUID.randomUUID().toString()}"
podTemplate(label: label,
	containers: [
        containerTemplate(name: 'python', image: 'python:3.11.5-alpine3.18', ttyEnabled: true, command: 'cat')
  ]) {

    node(label) {
        stage('python') {
            container('python') {
                    sh 'python --version'
            }
        }
    }
}
```



#### 5.4 podman JenkinsFile

```groovy
def label = "hello-${UUID.randomUUID().toString()}"
podTemplate(label: label,
	containers: [
        containerTemplate(name: 'podman', image: 'mattermost/podman:1.8.0', ttyEnabled: true, command: 'cat', privileged:true)
  ]) {

    node(label) {
        container('podman') {
	        stage('build') {   
                    sh 'podman version'
            }
        }
    }
}
```



### Docker? Podman? 

Podman은 Pod Manager Tool 이라는 의미로 OCI  컨테이너를 개발하고 관리하고 실행하도록 도와주는 컨테이너 엔진이다. Red Hat Enterprise Linux 8 과 CentOS8부터는 Docker 대신 Podman을 제공하고 있다. 



Docker는 애플리케이션을 빌드하고 컨테이너화하기 위한 **Docker Engine**, 컨테이너 이미지를 배포하고 제공하기 위한 Docker Registry, 여러개의 컨테이너 애플리케이션을 정의하고 실행하기 위한 Docker Compose, 사용자의 로컬 컴퓨터나 클라우드 인스턴스에 도커 호스트를 구성해주는 Docker Machine, 컨테이너 클러스터링 및 스케줄링을 위한 Docker Swarm으로 구성됩니다. **Podman은 Docker의 핵심 기능인 Docker Engine 기능과 유사하다고 볼 수 있습니다**. 



![img](asset/jenkins/i11mg.png)



Docker Engine에는 **컨테이너 이미지를 관리하고, 컨테이너 이미지를 이용해 컨테이너를 실행하기 위한 Docker daemon**이 있습니다. 그리고, **이미지와 컨테이너를 사용자가 관리하고 사용할 수 있도록 커맨드 기반으로 된 Docker Client**가 있습니다. Podman 역시 컨테이너를 실행하기 위한 **컨테이너 이미지, 그리고, 이미지를 통해 실행된 컨테이너와 이를 사용하기 위한 커맨드 기반의 유틸리티**가 있습니다. 



![img](asset/jenkins/im3333g.png)



Docker는 컨테이너 레지스트리로부터 이미지를 받아와 Docker 내부의 이미지 저장소에 저장을 합니다. 그리고, 이미지 저장소에 저장된 이미지를 이용하여 컨테이너를 실행할 수 있으며, 실행 중인 컨테이너를 이미지로 빌드할 수도 있습니다. Docker는 이런 **다양한 작업들을 Docker daemon을 통해 수행**합니다. 그렇기 때문에 데몬에 문제가 발생했을 경우 모든 컨테이너와 이미지에 영향이 가며, 커맨드 명령어로 컨테이너를 제어할 때도 영향을 미칩니다. 



![img](asset/jenkins/im2222222g.png)



반면에 **Podman은 daemon 없이 커맨드로 컨테이너 레지스트리로부터 이미지를 받아와 Podman 호스트의 로컬 이미지 저장소에 이미지를 저장하고, 해당 이미지를 이용하여 컨테이너를 실행**합니다. 이때 podman 라이브러리를 통해 바로 컨테이너를 실행하기 때문에 컨테이너 간에 서로 영향을 주지 않으며, 컨테이너와 이미지 사이, 커맨드 명령어로 컨테이너를 제어하거나 이미지를 관리할 때도 서로 영향을 주지 않습니다.



**즉 Podman은 컨테이너 및 컨테이너 이미지를 실행하고 관리 할 수 있습니다. 그리고 docker와 동일한 기능과 명령 옵션의 대부분을 지원하며** 

**차이점은 podman은 docker 또는 다른 활성 컨테이너 런타임이 필요하지 않다는 것이다.**



**kubernetes docker 지원 중단이슈**

kubernetes의 default container runtime 이었던 docker 가 v1.20 부터 deprecated 되었으며 docker를 지원하지 않는 이유는 

docker가 **CRI(Container Runtime Interface)** 를 구현하지 않았고, kubernetes에서 dockershim으로 변환하는데 비효율적이라고 판단했기 때문이다 

즉 좀더 자세히 말하자면 kubernetes는 docker 지원을 중단한게 아니라**Kubelet에서 docker-shim의 지원이 Deprecation 되었다고 보는게맞다.**

(https://ikcoo.tistory.com/189)





---



### 6. Jenkins CI 수행   



#### Maven(java) 빌드용 JenkinsFile

```groovy
def label = "hello-${UUID.randomUUID().toString()}"
podTemplate(label: label,
	containers: [
        containerTemplate(name: 'maven', image: 'nexus-repo.ssongman.duckdns.org/user02/maven-build-tool:1.0.0', ttyEnabled: true, command: 'cat'),
        containerTemplate(name: 'podman', image: 'nexus-repo.ssongman.duckdns.org/user02/build-tool:1.0.0', ttyEnabled: true, command: 'cat', privileged:true)
  ]) {
    node(label) {
        stage('Get Source') {
            container('maven') {
                sh"""
                git clone http://user02:${GIT_TOKEN}@gitlab.35.209.207.26.nip.io/user02/base-project.git
                cd base-project/sample/hello-world-spring/demo
                mvn clean package 
                """
            }
        }
        stage('Build & push') {
            container('podman') {
                    sh """
                    cd base-project/sample/hello-world-spring/demo
                    podman login -u ${NEXUS_USERNAME} -p ${NEXUS_PASSWORD} ${NEXUS_HOST} --tls-verify=false
                    podman build -t ${NEXUS_HOST}/user02/spring-jenkins:1.0.0 --cgroup-manager=cgroupfs --tls-verify=false . 
                    podman push ${NEXUS_HOST}/user02/spring-jenkins:1.0.0  --tls-verify=false
                    """
            }
        }
    }
}

```

빌드 결과

![image-20230923014502459](asset/jenkins/image-20230923014502459.png)



---



#### Npm(node) 빌드용 JenkinsFile

```groovy
def label = "hello-${UUID.randomUUID().toString()}"
podTemplate(label: label,
	containers: [
        containerTemplate(name: 'npm', image: 'nexus-repo.ssongman.duckdns.org/user02/npm-build-tool:1.0.0', ttyEnabled: true, command: 'cat'),
        containerTemplate(name: 'podman', image: 'nexus-repo.ssongman.duckdns.org/user02/build-tool:1.0.0', ttyEnabled: true, command: 'cat', privileged:true)
  ]) {
    node(label) {
        stage('Get Source') {
            container('npm') {
                sh"""
                git clone http://user02:${GIT_TOKEN}@gitlab.35.209.207.26.nip.io/user02/base-project.git
                """
            }
        }
        stage('Build & push') {
            container('podman') {
                    sh """
                    cd base-project/sample/hello-world-express
                    podman login -u ${NEXUS_USERNAME} -p ${NEXUS_PASSWORD} ${NEXUS_HOST} --tls-verify=false
                    podman build -t ${NEXUS_HOST}/user02/express-jenkins:1.0.0 --cgroup-manager=cgroupfs --tls-verify=false . 
                    podman push ${NEXUS_HOST}/user02/express-jenkins:1.0.0  --tls-verify=false
                    """
            }
        }
    }
}
```

빌드결과

![image-20230923014554545](asset/jenkins/image-20230923014554545.png)



---



#### Flask(python) 빌드용 JenkinsFile 

```groovy
def label = "hello-${UUID.randomUUID().toString()}"

podTemplate(label: label,
	containers: [
        containerTemplate(name: 'flask', image: 'nexus-repo.ssongman.duckdns.org/user02/python-build-tool:1.0.0', ttyEnabled: true, command: 'cat'),
        containerTemplate(name: 'podman', image: 'nexus-repo.ssongman.duckdns.org/user02/build-tool:1.0.0', ttyEnabled: true, command: 'cat', privileged:true)
  ]) {
    node(label) {
        stage('Get Source') {
            container('flask') {
                sh"""
                git clone http://user02:${GIT_TOKEN}@gitlab.35.209.207.26.nip.io/user02/base-project.git
                """
            }
        }
        stage('Build & push') {
            container('podman') {
                    sh """
                    cd base-project/sample/hello-world-flask
                    podman login -u ${NEXUS_USERNAME} -p ${NEXUS_PASSWORD} ${NEXUS_HOST} --tls-verify=false
                    podman build -t ${NEXUS_HOST}/user02/flask-jenkins:1.0.0 --cgroup-manager=cgroupfs --tls-verify=false . 
                    podman push ${NEXUS_HOST}/user02/flask-jenkins:1.0.0  --tls-verify=false
                    """
            }
        }
    
    }
}

```

빌드결과

![image-20230923020622987](asset/jenkins/image-20230923020622987.png)



---



### 7. 다양한 옵션 사용

#### 7.1 Schedule

![image-20230916210149032](asset/jenkins/image-20230916210149032.png)

Jenkins 의 Schedule 기능은 Linux 의 Crontab 형식을 취한다.

**Cron 이란?**

cron은 유닉스계열 컴퓨터 운영체제의 시간 기반 잡 스케줄러이다. 

소프트웨어 환경을 설정하고 관리하는 사람들은 작업을 고정된 시간, 날짜, 간격에 주기적으로 실행할 수 있도록 스케줄링하기 위해 cron을 사용한다.

```
# 매일 낮 2시 15분에 실행
15 14 * * *

# 매월 5일 새벽 한시 실행 
00 01 5 * *

# 3월동안 6시에 실행
00 06 * 3 *

# 일요일마다 새벽1시 실행
00 01 * * 7
월 : 1, 화 : 2 ..... 일 : 7

# 15분마다 실행
*/15 * * * *

# 2시 ~ 4시 동안 10분마다 실행
*/10 2-4 * * *

# 5시, 9시에 실행
* 5,9 * * *


분 | 시간 | 날짜 | 월 | 요일 | 명령  순서
```



---



#### 7.2 WebHook

**Jenkins Pipeline 설정**

**Build Triggers -> BuildWhen a change pushed to GitLab** 

**고급 -> Secret token -> Generate**

![image-20230923021138257](asset/jenkins/image-20230923021138257.png)

Build Triggers -> Build When ~!~~고급 토큰발급

![image-20230923021507892](asset/jenkins/image-20230923021507892.png)

Add new webhook

![image-20230923021406848](asset/jenkins/image-20230923021406848.png)

**Gitlab Proejct Settings -> Webhook -> URL/Token  입력**

![image-20230923021727138](asset/jenkins/image-20230923021727138.png)



Push 요청 -> Jenkins Build 확인

![image-20230923021925981](asset/jenkins/image-20230923021925981.png)



---



#### 7.3 기본 옵션

**General**

Do not allow concurrent builds

```
동시 빌드 수행을 방지
```

Do not allow the pipeline to resume if the controller restarts

```
젠킨스가 재기동되었을때 자동 재개를 방지
```

Pipeline speed/durability override

```
파이프라인의 속도,지속성을 설정
```

Throttle builds

```
빌드가 동시에 여러번 호출되는걸 막는 작업
```



**Trigger**

Build after other projectes are built

```
다른 프로젝트의 빌드가 완료되면 이 프로젝트에 대한 새 빌드가 예약되도록 트리거를 설정
```

Quit Period

```
0이 아닌 경우 새로 트리거된 빌드가 대기열에 추가되지만  빌드를 시작하기 전 지정된 시간(초) 대기
```

Build preiodically

```
빌드 주기 지정
```

PollSCM

```
SCM에서 변경 사항을 폴링하도록 Jenkins를 구성
```

빌드 원격 유발

```
미리 정의된 특수 URL(스크립트에 편리함)에 액세스하여 새 빌드를 트리거
```



---



#### 7.4 다양한 플러그인

https://plugins.jenkins.io/



출처

```
https://arisu1000.tistory.com/27848
https://blog.voidmainvoid.net/140
https://narup.tistory.com/179
localhost:8085/albums/1
https://jamesdefabia.github.io/docs/user-guide/kubectl/kubectl_create_secret_docker-registry/
https://nodejs.org/ko/docs/guides/nodejs-docker-webapp
https://docs.docker.com/engine/reference/builder/
https://github.com/datawire/hello-world-python
https://www.jenkins.io/doc/book/installing/kubernetes/
https://hungc.tistory.com/79
https://help.iwinv.kr/manual/read.html?idx=582
https://velog.io/@borab/%EC%BF%A0%EB%B2%84%EB%84%A4%ED%8B%B0%EC%8A%A4-docker-%EC%A7%80%EC%9B%90-%EC%A4%91%EB%8B%A8%EC%97%90-%EB%94%B0%EB%A5%B8-%EB%8C%80%EC%95%88
https://naleejang.tistory.com/227
https://ikcoo.tistory.com/189
```

[github.com/helm/helm/releases](https://github.com/helm/helm/releases)
