# Automated AWS Infrastructure with Security & Backup
test
**Team Project (5 members)**  
<img width="50" alt="Image" src="https://github.com/user-attachments/assets/15f851ae-b285-434b-8057-c523f60bf50e" /> Infra 부트캠프 과정에서 진행된 팀 프로젝트입니다.

## ◆ Architecture Overview

Terraform과 Ansible을 활용하여 AWS 인프라를 코드 기반으로 자동화하고, Kubernetes 환경에 애플리케이션을 배포했습니다.  
또한 보안 그룹 설계와 WAF 적용, RDS 자동 백업 구성 및 장애 시 RDS 복구 기능을 구현했습니다.  
VictoriaMetrics와 Grafana 기반 모니터링을 구축하여,  
수동 콘솔 작업 없이 쉽게 재현 가능한 안정적인 클라우드 아키텍처를 제공합니다.

### Tech Stack

![AWS](https://img.shields.io/badge/-AWS-232F3E?style=flat&logo=amazon-aws&logoColor=white)
![Amazon Linux 2023](https://img.shields.io/badge/-Amazon%20Linux%202023-232F3E?style=flat&logo=linux&logoColor=white)
![Terraform](https://img.shields.io/badge/-Terraform-623CE4?style=flat&logo=terraform&logoColor=white)
![Ansible](https://img.shields.io/badge/-Ansible-000000?style=flat&logo=ansible&logoColor=white)
![Kubernetes](https://img.shields.io/badge/-Kubernetes-326CE5?style=flat&logo=kubernetes&logoColor=white)
![MySQL](https://img.shields.io/badge/-MySQL-4479A1?logo=mysql&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/-GitHub_Actions-2088FF?style=flat&logo=github-actions&logoColor=white)
![VictoriaMetrics](https://img.shields.io/badge/-VictoriaMetrics-621773?logo=victoriametrics&logoColor=white)
![Grafana](https://img.shields.io/badge/-Grafana-F46800?style=flat&logo=grafana&logoColor=white)

## ◆ Infrastructure Architecture

<img width="3984" height="2244" alt="Architecture Diagram" src="https://github.com/user-attachments/assets/7cfe18bb-944f-43e6-8827-aeb47d8d87d7" />

### Network & Security

- GitHub Secrets 기반 민감 정보 관리
- Kubernetes Ingress Controller에 WAF(ModSecurity)를 적용하여 웹 애플리케이션 보호
- Amazon RDS를 Private Subnet에 배치하여 외부 직접 접근 차단
- EC2 ↔ RDS 간 Security Group 참조 방식 적용

### Backup Strategy

- Amazon RDS 자동 백업 및 스냅샷 정책 구성
- 장애 발생 시 RDS 복구 가능 (스냅샷 기반)
- Terraform / Ansible과 연동되어 RDS 복구 후 EC2와 자동 연결
- 재해 복구와 데이터 안정성을 위한 백업 전략 설계

### Infrastructure as Code

- Terraform 모듈 분리 및 단계적 배포로 State 충돌 방지  
  - Branch: `feature/sg`, `feature/rds`, `main`, `rds-dr`
- GitHub Actions 기반 인프라 자동 배포 파이프라인 구축
- RDS 복구 시 인프라 코드 덕분에 EC2와 자동 연결 지원

### Monitoring

<img width="1833" height="923" alt="Monitoring Dashboard" src="https://github.com/user-attachments/assets/26796d76-e5fe-4f47-8429-17af2c3834a8" />

- VictoriaMetrics 기반 메트릭 수집
- Grafana 대시보드 구성
- Kubernetes Pod / Service 상태 모니터링
- 웹 트래픽 및 공격 패턴 시각화

## ◆ Deployment Process

1. **Security Group 배포**

   ```
   feature/sg 브랜치 → GitHub Actions Workflow 실행 → Security Group 생성
   ```
   
2. **RDS 배포**

   ```
   feature/rds 브랜치 → GitHub Actions Workflow 실행 → RDS 인스턴스 생성
   ```

3. **애플리케이션 및 인프라 배포**

   ```
   main 브랜치 → GitHub Actions Workflow 실행 → EC2 / Kubernetes / 애플리케이션 배포
   ```

4. **웹 서비스 접속**

   ```
   http://<PUBLIC_IPV4_ADDRESS>
   ```

5. **RDS 복구**
   ```
   rds-dr 브랜치 → GitHub Actions Workflow 실행 → RDS 복구
   ```

## ◆ Resource Cleanup Guide

> [!NOTE]
> 불필요한 비용 발생을 방지하기 위해 리소스 삭제 시 아래 순서를 따릅니다.

1. EC2 인스턴스 종료

2. RDS 삭제
   - RDS 인스턴스 삭제
   - 생성된 스냅샷 삭제
   - DB Subnet Group 삭제

3. VPC 리소스 삭제
   - Private Subnet
   - Route Table

4. Security Group 삭제

## ◆ Key Features

- Terraform 기반 AWS 인프라 자동화 (Infrastructure as Code)
- GitHub Actions 기반 CI/CD 파이프라인 구축
- Kubernetes(K3s) 애플리케이션 배포 환경 구성
- WAF(ModSecurity) 기반 웹 애플리케이션 보안 강화
- VictoriaMetrics / Grafana 기반 모니터링 시스템 구축
- Amazon RDS 자동 백업 및 장애 시 복구 지원




