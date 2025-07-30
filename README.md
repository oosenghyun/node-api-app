📦 EKS 기반 Node.js 클라우드 배포 프로젝트 - README

✨ 개요

이 프로젝트는 Node.js 애플리케이션을 AWS의 EKS(Elastic Kubernetes Service)에 배포하여,
실제 클라우드 환경에서의 컨테이너 오케스트레이션, CI/CD 자동화, 인프라 구성 등을 실습하는 실무형 포트폴리오입니다.

📚 사용 기술 스택

Node.js (Express): 백엔드 애플리케이션

Docker: 애플리케이션 컨테이너화

Amazon ECR: Docker 이미지 저장소

Amazon EKS: Kubernetes 클러스터 관리

kubectl / eksctl: 클러스터 및 리소스 관리

Helm: Kubernetes 패키지 매니저 (Ingress Controller 설치 등)

ALB (Application Load Balancer): 외부 트래픽 라우팅

IAM + IRSA: Kubernetes 서비스 계정에 IAM 역할 연결

GitHub Actions: CI/CD 자동화 파이프라인

🧩 디렉토리 구조

node-api-app/
├── app.js
├── Dockerfile
├── package.json
├── .github/
│   └── workflows/
│       └── deploy.yml
└── k8s/
    ├── deployment.yaml
    ├── service.yaml
    └── ingress.yaml

🚀 배포 과정 요약

1️⃣ Node.js 앱 개발 및 Dockerize

app.js 파일 작성

Dockerfile로 컨테이너 이미지 빌드

2️⃣ ECR에 이미지 푸시

docker buildx build --platform linux/amd64 -t <ECR_URL>/node-api-app:latest --push .

3️⃣ EKS 클러스터 생성

eksctl create cluster \
--name node-app-cluster \
--region ap-northeast-2 \
--nodegroup-name node-group \
--node-type t3.medium \
--nodes 2 \
--nodes-min 1 \
--nodes-max 3 \
--managed

4️⃣ Kubernetes 매니페스트 배포

kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl apply -f ingress.yaml

5️⃣ ALB Ingress Controller + IRSA 설정

Helm으로 aws-load-balancer-controller 설치

IAM Policy 생성 및 IRSA(ServiceAccount) 연결

6️⃣ GitHub Actions로 CI/CD 자동화

.github/workflows/deploy.yml 작성

코드 push → Docker build → ECR push → kubectl rollout 자동화

7️⃣ CloudWatch 로그 수집 (예정)

Fluent Bit + IAM 설정을 통해 CloudWatch Logs에 파드 로그 수집 예정

🌍 최종 결과

외부에서 ALB 주소를 통해 Node.js 앱에 접근 가능

GitHub Actions로 자동 배포 가능

EKS 내 Kubernetes 리소스 배포 확인 완료

📝 느낀 점 (포트폴리오에 기재용 예시)

AWS 기반 DevOps 환경에서 CI/CD 파이프라인을 직접 구성하고,
인프라와 배포 자동화를 실습하며 클라우드 개발자로서의 역량을 실전처럼 다질 수 있었습니다.

⚠️ 리소스 정리 (과금 방지)

# 클러스터 삭제
eksctl delete cluster --name node-app-cluster --region ap-northeast-2

ECR, IAM Role, ALB, Security Group 등도 수동 정리 필요

