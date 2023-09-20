# ktds-edu-cloud-cicd
클라우드 AP 소스코드 형상관리 전략 및 CI/CD 실전





# Cloud AP 소스코드 형상관리 전략 및 CI/CD 실전





# 1. 시작하기전에 ( [문서 보기](./beforebegin/beforebegin.md) )





# 2. 12 Factor 방법론 ( [문서 보기](./cloud-branch/12factors.md) )

- 12 Factor 방법론이란?
- 왜 12 Factor 방법론을 사용해냐 하나?
- 12 Factor 상세





# 3. Git Flow Branch 전략 ( [문서 보기](./cloud-branch/gitflow-branch.md) )

- 소스코드 형상관리 및 Branch 전략
- gitlab 샘플 프로젝트 이해 및 분기
  - 실습
  - git clone
  - git commit push
  - branch checkout



# 4. Jenkins를 이용한 CI 구성 및 실습 ( [문서 보기](./jenkins/jenkins.md) )
- Jenkins CI 역할 pipeline 이해
  - git clone
  - jib ...

- Helm Chart 를 활용한 Jenkins 설치 실습(Local)
- Pipeline 실습(Cloud)



# 5. ArgoCD를 이용한 CD 구성 및 실습 ( [문서 보기](./argocd/argocd.md) )
- GitOps 및 GitFlow 이해 및 셋팅
- Helm Chart 를 활용한 ArgoCD 설치 실습(Local)
- ArgoCD 배포 실습(Cloud) 



#  별첨. EduCluster Solution Setup ( [문서 보기](./cluster-setup/cluster-setup.md) )

## 1) GitLab Setup

- kubernetes Install (k3s)
- Helm Install
- 기타 Tool Setup

## 2) Kafka Setup(Strimzi) on Cloud

- Strimzi Cluster Operator Install
- Kafka Cluster 생성
- KafkaUser / KafkaTopic 생성
- Monitoring 환경구축 (Kafka Exporter / Prometheus / Grafana / Kafdrop )

## 3) Redis on Cloud

- Redis Cluster Install
- Web UI (P3X / RedisInsight)

