# Guardians-Infra: GitOps 기반 하이브리드 클라우드 인프라 구성

## 🔧 프로젝트 개요

**Guardians-Infra**는 GitOps 방식으로 클라우드 클러스터와 온프레미스 클러스터를 통합 관리하는 **하이브리드 Kubernetes 인프라**입니다. 모든 설정은 Git 저장소에 선언적으로 저장되며, ArgoCD를 통해 자동으로 배포 및 동기화됩니다.

---

## 🧱 인프라 구성

### 📦 주요 스택

| 컴포넌트            | 설명                         |
| ------------------- | ---------------------------- |
| **Spring Boot**     | 백엔드 서비스                |
| **React (Vite)**    | 프론트엔드 서비스            |
| **Nginx Ingress**   | 진입 트래픽 라우팅           |
| **Grafana**         | 대시보드 시각화              |
| **Prometheus**      | 모니터링 수집기              |
| **Promtail + Loki** | 로그 수집 및 저장            |
| **Jenkins**         | CI/CD 파이프라인             |
| **Vault**           | 시크릿 관리 및 주입          |
| **PostgreSQL**      | 관계형 데이터베이스          |
| **Redis**           | 캐시 및 세션 저장소          |
| **Harbor**          | 프라이빗 이미지 레지스트리   |
| **Rook-Ceph**       | 고가용성 퍼시스턴트 스토리지 |

---

## ☁️ 클러스터 구성

### 🟦 Cloud Cluster

- 위치: AWS EC2 기반 K8s 클러스터
- 목적: 애플리케이션 및 웹 환경 호스팅
- 주요 컴포넌트:

  - `frontend/` – React 기반 UI
  - `backend/` – Spring Boot 기반 API 서버
  - `monitoring/` – Prometheus, Grafana, Promtail
  - `nginx-ingress/` – Ingress Controller

### 🏠 On-Premise Cluster

- 위치: 온프레미스 VM 기반 K8s 클러스터
- 목적: 데이터/운영 인프라 및 보안 시스템 운영
- 주요 컴포넌트:

  - `harbor/` – 프라이빗 레지스트리
  - `jenkins/` – CI 파이프라인
  - `monitoring/` – Grafana, Prometheus
  - `nginx-ingress/` – 내부 진입점
  - `postgresql/`, `redis/`, `vault/` – 핵심 서비스
  - `rook-ceph/` – 스토리지 백엔드 (Ceph 기반)

---

## 🧭 GitOps 디렉터리 구조

```
guardians-infra/
├── argocd-apps/ # ArgoCD 애플리케이션 선언
│   ├── root-app.yaml # 모든 앱을 트리 형식으로 관리
│   ├── backend.yaml
│   ├── frontend.yaml
│   ├── nginx-ingress.yaml
│   ├── cloud-prometheus.yaml
│   ├── cloud-promtail.yaml
│   ├── onpremise-nginx.yaml
│   ├── postgresql.yaml
│   ├── redis.yaml
│   ├── harbor.yaml
│   ├── jenkins.yaml
│   ├── vault.yaml
│   └── rook-ceph.yaml
├── cloud-cluster/ # 클라우드 클러스터 앱 리소스
│   ├── backend/
│   ├── frontend/
│   ├── monitoring/
│   └── nginx-ingress/
├── onpremise-cluster/ # 온프레미스 클러스터 앱 리소스
│   ├── harbor/
│   ├── jenkins/
│   ├── monitoring/
│   ├── nginx-ingress/
│   ├── postgresql/
│   ├── redis/
│   ├── rook-ceph/
│   └── vault/
└── README.md
```

---

## 🚀 GitOps Workflow

1. 모든 클러스터 구성 및 앱 설정은 Git 저장소에 선언적으로 관리됩니다.
2. ArgoCD는 `argocd-apps/root-app.yaml`을 기준으로 모든 앱을 계층적으로 동기화합니다.
3. 변경 사항은 Git 커밋 → ArgoCD 자동 배포 → 클러스터 반영 흐름으로 이루어집니다.

---

## 🛡️ 보안 및 고가용성 전략

- **Vault + AppRole**: Spring 앱에 비밀 자동 주입
- **Rook-Ceph**: 다중 노드 기반 고가용성 스토리지 구성
- **Harbor**: 이미지 스캔과 인증 기반 private registry 운영
- **NetworkPolicy**: 칼리-Pod에서 워게임-Pod로만 통신 허용
- **Prometheus + Grafana + Loki**: 모니터링 및 로그 수집 시각화

---

## 📘 향후 계획

- ArgoCD Notifications 연동 (Slack)
- GitHub Actions 기반 CI와 연계
- 사용자 워게임 환경 자동 생성 및 삭제 자동화 고도화

---

> 이 저장소는 `Guardians` 팀의 하이브리드 클라우드 기반 보안 훈련 플랫폼 인프라를 GitOps 방식으로 관리합니다.
