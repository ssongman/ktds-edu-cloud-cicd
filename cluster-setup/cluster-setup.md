# < EduCluster Server Setup >







# 1. K3S 구성





## 1) K3S 구성(HA mode)

### (1) master node

```sh
# root 권한으로 수행
$ su


# master01에서
$ curl -sfL https://get.k3s.io | sh -s - --write-kubeconfig-mode 644 --cluster-init

# 확인
$ kubectl version
Client Version: version.Info{Major:"1", Minor:"26", GitVersion:"v1.26.4+k3s1", GitCommit:"8d0255af07e95b841952563253d27b0d10bd72f0", GitTreeState:"clean", BuildDate:"2023-04-20T00:33:18Z", GoVersion:"go1.19.8", Compiler:"gc", Platform:"linux/amd64"}
Kustomize Version: v4.5.7
Server Version: version.Info{Major:"1", Minor:"26", GitVersion:"v1.26.4+k3s1", GitCommit:"8d0255af07e95b841952563253d27b0d10bd72f0", GitTreeState:"clean", BuildDate:"2023-04-20T00:33:18Z", GoVersion:"go1.19.8", Compiler:"gc", Platform:"linux/amd64"}
---
WARNING: This version information is deprecated and will be replaced with the output from kubectl version --short.  Use --output=yaml|json to get the full version.
Client Version: version.Info{Major:"1", Minor:"26", GitVersion:"v1.26.4+k3s1", GitCommit:"8d0255af07e95b841952563253d27b0d10bd72f0", GitTreeState:"clean", BuildDate:"2023-04-20T00:33:18Z", GoVersion:"go1.19.8", Compiler:"gc", Platform:"linux/amd64"}
Kustomize Version: v4.5.7
Server Version: version.Info{Major:"1", Minor:"26", GitVersion:"v1.26.4+k3s1", GitCommit:"8d0255af07e95b841952563253d27b0d10bd72f0", GitTreeState:"clean", BuildDate:"2023-04-20T00:33:18Z", GoVersion:"go1.19.8", Compiler:"gc", Platform:"linux/amd64"}





# IP/ token 확인
$ cat /var/lib/rancher/k3s/server/node-token
K1053500b81d3608e011e82c285ad06590811db8059cca5ca0b3cbd10e969f95e03::server:9bd15fea5819e2ce185669abd5a1ce48
---
K10b0a3c6320b9ceff2b14ea265774c388e7aa4ad8cfa35804efb98eb9235f99290::server:5e7fd6a284e028bdcba918abd9fae74e




# master01 에서
$ kubectl get nodes -w

# master02, 03 에서
$ export MASTER_TOKEN="K10b0a3c6320b9ceff2b14ea265774c388e7aa4ad8cfa35804efb98eb9235f99290::server:5e7fd6a284e028bdcba918abd9fae74e"
  export MASTER_IP="10.128.0.35"

$ curl -sfL https://get.k3s.io | sh -s - --write-kubeconfig-mode 644 --server https://${MASTER_IP}:6443 --token ${MASTER_TOKEN}

…
[INFO]  systemd: Starting k3s-agent   ← 정상 로그




# master01 에서
$ kubectl get nodes
NAME                STATUS   ROLES                       AGE     VERSION
ktds-k3s-master01   Ready    control-plane,etcd,master   2m30s   v1.26.4+k3s1
ktds-k3s-master02   Ready    control-plane,etcd,master   29s     v1.26.4+k3s1
ktds-k3s-master03   Ready    control-plane,etcd,master   46s     v1.26.4+k3s1



# [참고]istio setup을 위한 k3s 설정시 아래 참고
## traefik 을 deploy 하지 않는다. 
## istio 에서 별도 traefic 을 설치하는데 이때 기설치된 controller 가 있으면 충돌 발생함 - istio version 1.14
$ curl -sfL https://get.k3s.io |INSTALL_K3S_EXEC="--no-deploy traefik" sh -

# [참고] istio version 1.14 에서 현재 1.17.2로 버전업되면서 DaemonSet 이 사라졌다.
# traefik 과 충돌 현상도 사라졌다.
```





#### [참고] 수동실행

설치가 안된다면 아래와 같이 수동실행 진행해 보자.

```sh
# root 권한으로

$ export MASTER_TOKEN="K10b0a3c6320b9ceff2b14ea265774c388e7aa4ad8cfa35804efb98eb9235f99290::server:5e7fd6a284e028bdcba918abd9fae74e"
  export MASTER_IP="10.128.0.35"


$ sudo k3s server --server https://${MASTER_IP}:6443 --token ${MASTER_TOKEN} &


$ k3s server &
…
COMMIT 
…

# k3s 데몬 확인
$ ps -ef|grep k3s
root         590     405  0 13:05 pts/0    00:00:00 sudo k3s server
root         591     590 76 13:05 pts/0    00:00:26 k3s server
root         626     591  5 13:05 pts/0    00:00:01 containerd -c /var/lib/rancher/k3s/agent/etc/containerd/config.toml -a /run/k3s/containerd/containerd.sock --state /run/k3s/containerd --root /var/lib/rancher/k3s/agent/containerd
...

$ k3s kubectl version
WARNING: This version information is deprecated and will be replaced with the output from kubectl version --short.  Use --output=yaml|json to get the full version.
Client Version: version.Info{Major:"1", Minor:"26", GitVersion:"v1.26.4+k3s1", GitCommit:"8d0255af07e95b841952563253d27b0d10bd72f0", GitTreeState:"clean", BuildDate:"2023-04-20T00:33:18Z", GoVersion:"go1.19.8", Compiler:"gc", Platform:"linux/amd64"}
Kustomize Version: v4.5.7
Server Version: version.Info{Major:"1", Minor:"26", GitVersion:"v1.26.4+k3s1", GitCommit:"8d0255af07e95b841952563253d27b0d10bd72f0", GitTreeState:"clean", BuildDate:"2023-04-20T00:33:18Z", GoVersion:"go1.19.8", Compiler:"gc", Platform:"linux/amd64"}

```







#### [참고] Master Node 비정상일 경우

master node 중 하나가 비정상 작동한다면 아래와 같이 수행하자.

* 시나리오1
  * 가정
    * master1번이 비정상, master2,3 은 정상일 경우
  * 조치
    * token은 최초 공유했던 token 값으로 유지한다.
    * master_ip 는 정상인 2번의 IP 를 참고한다.

```sh
# root 권한으로

# 1) k3s kill
$ sh /usr/local/bin/k3s-killall.sh

# 2) 환경변수 셋팅
# 정상인 MASTER_IP 를 셋팅한다.
$ export MASTER_TOKEN="K10b0a3c6320b9ceff2b14ea265774c388e7aa4ad8cfa35804efb98eb9235f99290::server:5e7fd6a284e028bdcba918abd9fae74e"
  export MASTER_IP="10.128.0.35"

# 3) k3s 실행
$ sudo k3s server --server https://${MASTER_IP}:6443 --token ${MASTER_TOKEN} &

…
COMMIT 
…

# 4) k3s 데몬 확인
$ ps -ef|grep k3s
root         590     405  0 13:05 pts/0    00:00:00 sudo k3s server
root         591     590 76 13:05 pts/0    00:00:26 k3s server
root         626     591  5 13:05 pts/0    00:00:01 containerd -c /var/lib/rancher/k3s/agent/etc/containerd/config.toml -a /run/k3s/containerd/containerd.sock --state /run/k3s/containerd --root /var/lib/rancher/k3s/agent/containerd
...

# 5) kubectl version 확인
$ k3s kubectl version
WARNING: This version information is deprecated and will be replaced with the output from kubectl version --short.  Use --output=yaml|json to get the full version.
Client Version: version.Info{Major:"1", Minor:"26", GitVersion:"v1.26.4+k3s1", GitCommit:"8d0255af07e95b841952563253d27b0d10bd72f0", GitTreeState:"clean", BuildDate:"2023-04-20T00:33:18Z", GoVersion:"go1.19.8", Compiler:"gc", Platform:"linux/amd64"}
Kustomize Version: v4.5.7
Server Version: version.Info{Major:"1", Minor:"26", GitVersion:"v1.26.4+k3s1", GitCommit:"8d0255af07e95b841952563253d27b0d10bd72f0", GitTreeState:"clean", BuildDate:"2023-04-20T00:33:18Z", GoVersion:"go1.19.8", Compiler:"gc", Platform:"linux/amd64"}

```











### (2) worker node

```sh
# root 권한으로 수행
# worker node 에서 각각 수행


$ export MASTER_TOKEN="K10b0a3c6320b9ceff2b14ea265774c388e7aa4ad8cfa35804efb98eb9235f99290::server:5e7fd6a284e028bdcba918abd9fae74e"
  export MASTER_IP="10.128.0.35"
  

$ curl -sfL https://get.k3s.io | K3S_URL=https://${MASTER_IP}:6443 K3S_TOKEN=${MASTER_TOKEN} sh -

…
[INFO]  systemd: Starting k3s-agent   ← 나오면 정상



# master01 에서
$ kubectl get nodes
NAME                STATUS   ROLES                       AGE     VERSION
ktds-k3s-master01   Ready    control-plane,etcd,master   5m19s   v1.26.4+k3s1
ktds-k3s-master02   Ready    control-plane,etcd,master   3m18s   v1.26.4+k3s1
ktds-k3s-master03   Ready    control-plane,etcd,master   3m35s   v1.26.4+k3s1
ktds-k3s-worker01   Ready    <none>                      28s     v1.26.4+k3s1
ktds-k3s-worker02   Ready    <none>                      63s     v1.26.4+k3s1
ktds-k3s-worker03   Ready    <none>                      53s     v1.26.4+k3s1


```



#### label 에 node-role 셋팅

```sh
# node role 셋팅
$ kubectl label node worker01 node-role.kubernetes.io/worker="true"
  kubectl label node worker02 node-role.kubernetes.io/worker="true"
  kubectl label node worker03 node-role.kubernetes.io/worker="true"
  kubectl label node worker04 node-role.kubernetes.io/worker="true"

# node role 확인
$ k get nodes
NAME                STATUS   ROLES                       AGE    VERSION
ktds-k3s-master01   Ready    control-plane,etcd,master   125d   v1.27.5+k3s1
ktds-k3s-master02   Ready    control-plane,etcd,master   125d   v1.26.4+k3s1
ktds-k3s-master03   Ready    control-plane,etcd,master   111d   v1.26.5+k3s1
worker01            Ready    worker                      41m    v1.27.6+k3s1
worker02            Ready    worker                      35m    v1.27.6+k3s1
worker03            Ready    worker                      39m    v1.27.6+k3s1
worker04            Ready    worker                      36m    v1.27.6+k3s1

```







#### [참고] 수동실행

설치가 안된다면 아래와 같이 수동실행 진행해 보자.

```sh
# root 권한으로

$ sudo k3s agent --server https://${MASTER_IP}:6443 --token ${NODE_TOKEN} &
```









### (3) node 제거

#### Cordon

현재 노드에 배포된 Pod은 그대로 유지하면서, 추가적인 Pod의 배포를 제한하는 명령어

```
$ kubectl uncordon <node-name>

$ kubectl cordon <node-name>
```

`kubectl drain` 혹은 `kubectl cordon` 명령어를 적용한 노드는 `SechedulingDisabled` 상태가 되어 더 이상 Pod이 scheduling되지 않는다.`kubectl uncordon`은 노드의 이러한 `SchedulingDisabled` 상태를 제거하여 노드에 Pod이 정상적으로 스케쥴링 될 수 있도록 복구하는 명령어이다.



#### Drain

노드에 존재하는 모든 Pod을 제거하여 노드를 비우고, Pod들을 다른 노드에 새롭게 스케쥴링하는 명령어
drain 과정에 cordon이 포함되어 있다고 볼 수 있다.

```sh
$ kubectl get nodes
NAME                STATUS   ROLES                       AGE    VERSION
ktds-k3s-master01   Ready    control-plane,etcd,master   100d   v1.26.4+k3s1
ktds-k3s-master02   Ready    control-plane,etcd,master   100d   v1.26.4+k3s1
ktds-k3s-master03   Ready    control-plane,etcd,master   86d    v1.26.5+k3s1
ktds-k3s-worker01   Ready    <none>                      100d   v1.26.4+k3s1
ktds-k3s-worker02   Ready    <none>                      86d    v1.26.5+k3s1
ktds-k3s-worker03   Ready    <none>                      86d    v1.26.5+k3s1
ktds-k3s-worker04   Ready    <none>                      21h    v1.27.4+k3s1




$ kubectl drain ktds-k3s-worker04 --ignore-daemonsets --delete-emptydir-data
$ kubectl drain ktds-k3s-worker03 --ignore-daemonsets --delete-emptydir-data
$ kubectl drain ktds-k3s-worker02 --ignore-daemonsets --delete-emptydir-data
$ kubectl drain ktds-k3s-worker01 --ignore-daemonsets --delete-emptydir-data





$ kubectl get nodes
NAME                STATUS                     ROLES                       AGE     VERSION
ktds-k3s-master01   Ready                      control-plane,etcd,master   64m     v1.26.4+k3s1
ktds-k3s-master02   Ready                      control-plane,etcd,master   17m     v1.26.4+k3s1
ktds-k3s-master03   Ready                      control-plane,etcd,master   9m37s   v1.26.4+k3s1
ktds-k3s-worker01   Ready                      control-plane,etcd,master   7m51s   v1.26.4+k3s1
ktds-k3s-worker02   Ready                      control-plane,etcd,master   7m37s   v1.26.4+k3s1
ktds-k3s-worker03   Ready                      control-plane,etcd,master   7m37s   v1.26.4+k3s1
ktds-k3s-worker04   Ready,SchedulingDisabled   control-plane,etcd,master   21h    v1.27.4+k3s1


```



#### Node Delete

```sh
# drain 작업 이후
$ kubectl delete node ktds-k3s-worker04
$ kubectl delete node ktds-k3s-worker03
$ kubectl delete node ktds-k3s-worker02
$ kubectl delete node ktds-k3s-worker01

```





### (4) k3s uninstall

```sh
## uninstall
$ sh /usr/local/bin/k3s-killall.sh
  sh /usr/local/bin/k3s-uninstall.sh
  
```





## 2) 기타 Tool Setup



### (1) apt install 

```sh
# root 로

$ apt update

$ apt install vim

$ apt install tree

$ apt install iputils-ping

$ apt install net-tools

$ apt install netcat

$ apt install unzip

$ apt install git

$ apt install podman
$ podman version
Version:      3.4.2
API Version:  3.4.2
Go Version:   go1.15.2
Built:        Thu Jan  1 09:00:00 1970
OS/Arch:      linux/amd64

```







# 2. Gitlab Setup





## 1) deployment and service

```sh

$ cat > 11.gitlab-deployment-svc.yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: gitlab
  labels:
    app: gitlab
spec:
  replicas: 1
  selector:
    matchLabels:
      app: gitlab
  template:
    metadata:
      labels:
        app: gitlab
    spec:
      containers:
      - name: gitlab
        image: gitlab/gitlab-ce:latest
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: gitlab-svc
spec:
  selector:
    app: gitlab
  ports:
  - name: http
    protocol: TCP
    port: 80
    #targetPort: 8181
  #type: NodePort
---

```



## 2) ingress

```sh


$ cat > 12.gitlab-ingress.yaml
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  annotations:
    kubernetes.io/ingress.provider: "traefik"
  labels:
    app: gitlab
    release: gitlab
  name: ingress-gitlab
spec:
  rules:
  - host: "gitlab.35.209.207.26.nip.io"
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: gitlab-svc
            port:
              number: 80
              
---

```





## 3) [참고] route

Openshift 를사용할 경우는 아래와 같이 Route 를 등록한다.

```sh

$ cat > 13.gitlab-route.yaml
---
apiVersion: route.openshift.io/v1
kind: Route
metadata:
  name: gitlab-route
spec:
  host: gitlab.apps.211-34-231-82.nip.io
  port:
    targetPort: http
  to:
    kind: Service
    name: gitlab-svc
    weight: 100
  wildcardPolicy: None         
---


```





## 4) 권한부여

```sh

# pod list 를 읽을 수 있는 권한이 있어야 한다.  그냥 cluster-admin 주자.
# cluster role binding 

$ cat > 10.gitlab-clusterrolebinding.yaml
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: devops-gitlab
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: default
  namespace: gitlab
---


```



## 5) 초기 root 패스워드

```sh
# log...

Set up the initial password
Introduced in Omnibus GitLab 14.0.

By default, Omnibus GitLab automatically generates a password for the initial administrator user account (root) and stores it to /etc/gitlab/initial_root_password for at least 24 hours. For security reasons, after 24 hours, this file is automatically removed by the first gitlab-ctl reconfigure.

—--

cat /etc/gitlab/initial_root_password

Password: 8NkH1b3j9Hp0EQtIyPoKOtYT8npuly0tXjfPzak5Ggc=

# 패스워드 변경 24시간 후 위 파일은 자동으로 삭제된다.



# 패스워드 변경

id : root
password : ktdspass!

```





## 6) 확인



```sh


http://gitlab.35.209.207.26.nip.io

id : root

password : ktdspass!



```











# 3. helm install

쿠버네티스에 서비스를 배포하는 방법이 다양하게 존재하는데 그중 대표적인 방법중에 하나가 Helm chart 방식 이다.



## 1) helm chart 와 Architecture

#### helm chart 의 필요성

일반적으로 Kubernetes 에 서비스를 배포하기 위해 준비되는 Manifest 파일은 정적인 형태이다. 따라서 데이터를 수정하기 위해선 파일 자체를 수정해야 한다. 잘 관리를 한다면 큰 어려움은 없겠지만, 문제는 CI/CD 등 자동화된 파이프라인을 구축해서 애플리케이션 라이프사이클을 관리할 때 발생한다.

보통 애플리케이션 이미지를 새로 빌드하게 되면, 빌드 넘버가 변경된다. 이렇게 되면 새로운 이미지를 사용하기 위해 Kubernetes Manifest의 Image도 변경되어야 한다. 하지만 Kubernetes Manifest를 살펴보면, 이를 변경하기 쉽지 않다. Image Tag가 별도로 존재하지 않고 Image 이름에 붙어있기 때문입니다. 이를 자동화 파이프라인에서 변경하려면, sed 명령어를 쓰는 등의 힘든 작업을 해야 한다.

Image Tag는 굉장히 단적인 예제이다. 이 외에 도 Configmap 등 배포시마다 조금씩 다른 형태의 데이터를 배포해야 할때 Maniifest 파일 방식은 너무나 비효율적이다. Helm Chart 는 이런 어려운 점을 모두 해결한 훌륭한 도구이다. 비단, 사용자가 개발한 AP 뿐아니라 kubernetes 에 배포되는 오픈소스 기반 솔루션들은 거의 모두 helm chart 를 제공한다.

Istio 도 마찬가지로 helm 배포를 위한 chart 를 제공해 준다.

#### Helm Architecture



![helm-architecure.png](cluster-setup.assets/helm-architecure-1695559618260-1.png)





## 2) helm client download

helm client 를 local 에 설치해 보자.

개인 Termimal 에서 아래 작업을 수행하자.

```bash
# root 권한으로 수행
$ su


## 임시 디렉토리를 하나 만들자.
$ mkdir -p ~/helm/
$ cd ~/helm/

# 다운로드
$ wget https://get.helm.sh/helm-v3.12.0-linux-amd64.tar.gz

# 압축해지
$ tar -zxvf helm-v3.12.0-linux-amd64.tar.gz

# 확인
$ ll linux-amd64/helm
-rwxr-xr-x 1 1001 docker 50597888 May 11 01:35 linux-amd64/helm*

# move
$ mv linux-amd64/helm /usr/local/bin/helm

# 확인
$ ll /usr/local/bin/helm*
-rwxr-xr-x 1 1001 docker 50597888 May 11 01:35 /usr/local/bin/helm*


# 일반유저로 복귀
$ exit


# 확인
$ helm version
version.BuildInfo{Version:"v3.12.0", GitCommit:"c9f554d75773799f72ceef38c51210f1842a1dea", GitTreeState:"clean", GoVersion:"go1.20.3"}


$ helm -n user02 ls
NAME    NAMESPACE       REVISION        UPDATED STATUS  CHART   APP VERSION
```



## 3) bitnami repo 추가

- 유명한 charts 들이모여있는 bitnami repo 를 추가후 nginx 를 배포해 보자.

```bash
# test# add stable repo
$ helm repo add bitnami https://charts.bitnami.com/bitnami

$ helm search repo bitnami
# bitnami 가 만든 다양한 오픈소스 샘플을 볼 수 있다.
NAME                                            CHART VERSION   APP VERSION     DESCRIPTION
bitnami/airflow                                 14.1.3          2.6.0           Apache Airflow is a tool to express and execute...
bitnami/apache                                  9.5.3           2.4.57          Apache HTTP Server is an open-source HTTP serve...
bitnami/appsmith                                0.3.2           1.9.19          Appsmith is an open source platform for buildin...
bitnami/argo-cd                                 4.7.2           2.6.7           Argo CD is a continuous delivery tool for Kuber...
bitnami/argo-workflows                          5.2.1           3.4.7           Argo Workflows is meant to orchestrate Kubernet...
bitnami/aspnet-core                             4.1.1           7.0.5           ASP.NET Core is an open-source framework for we...
bitnami/cassandra                               10.2.2          4.1.1           Apache Cassandra is an open source distributed ...
...

# 설치테스트(샘플: nginx)
$ helm -n user02 install nginx bitnami/nginx

$ ku get all
NAME                         READY   STATUS              RESTARTS   AGE
pod/nginx-68c669f78d-wgnp4   0/1     ContainerCreating   0          10s

NAME            TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
service/nginx   LoadBalancer   10.43.197.4   <pending>     80:32754/TCP   10s

NAME                    READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx   0/1     1            0           10s

NAME                               DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-68c669f78d   1         1         0       10s

# 간단하게 nginx 에 관련된 deployment / service / pod 들이 설치되었다.


# 설치 삭제
$ helm -n user02 delete nginx

$ ku get all
No resources found in user02 namespace.
```







