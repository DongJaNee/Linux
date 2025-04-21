# Red Hat Enterprise Linux Server for HPC Head Node, Premium

## 1. Red Hat Enterprise Linux Server for HPC
- **HPC (High Performance Computing)** 환경에 최적화된 RHEL 버전
- 과학, 공학, 데이터 분석 등에서 **병렬 처리**와 **대량 연산**을 지원
- 일반 RHEL보다 다음 기능이 강화됨:
  - MPI(MPI 라이브러리) 지원
  - 작업 스케줄러 연동 (예: Slurm, PBS)
  - 클러스터 관리 도구 포함

## 2. Head Node
- HPC 클러스터의 **중앙 제어 노드**
- 주요 역할:
  - 사용자 명령어 수신 및 처리
  - 작업 스케줄링 및 배포
  - 클러스터 노드 상태 모니터링
  - 공통 소프트웨어의 배포 및 관리
- 클러스터 전체를 조율하는 **중추 서버**

## 3. Premium
- Red Hat의 **가장 높은 수준의 기술 지원 플랜**
- 특징:
  - 연중무휴(24x7) 기술 지원
  - 빠른 응답 시간 보장
  - 심화 기술 지원 가능 (예: 성능 튜닝, 복잡한 문제 해결)
  - 보안 패치, 시스템 업데이트 제공


## 다운로드 및 운용 방법

## 1. 사전 준비
- [Red Hat 공식 사이트](https://www.redhat.com)에서 계정 생성
- **유효한 서브스크립션** 필요 (HPC Head Node, Premium 라이선스 포함)
- 설치 대상 시스템의 요구 사양 확인

## 2. 설치 ISO 다운로드
1. [Red Hat Customer Portal](https://access.redhat.com) 접속
2. 상단 메뉴에서 **Downloads > Red Hat Enterprise Linux** 선택
3. **RHEL 버전 선택 (예: 9.x 또는 8.x)**  
4. "HPC용 에디션"이 별도로 표시되지 않는 경우, 일반 RHEL ISO를 다운로드 후 HPC 구성

> **참고:** HPC Head Node 전용 ISO는 일반적으로 제공되지 않으며, RHEL 설치 후 HPC 패키지를 추가로 구성함

## 3. 설치
- 다운로드한 ISO를 부팅 가능한 USB/DVD로 제작 (예: [Rufus](https://rufus.ie), `dd` 명령어 사용)
- 서버에 부팅 매체를 삽입 후 RHEL 설치 진행
- 설치 중, **서브스크립션 등록** 단계에서 유효한 계정 및 라이선스 정보 입력

## 4. HPC Head Node 구성
```bash
# 기본 패키지 설치
sudo dnf groupinstall "Development Tools"
sudo dnf install openmpi openmpi-devel environment-modules

# Slurm 스케줄러 예시 (선택)
sudo dnf install slurm slurm-slurmd slurm-slurmctld munge
## Red Hat Enterprise Linux Server for HPC Compute Node, Self-support
```

## 1. 개요
- **Red Hat Enterprise Linux for HPC Compute Node**는 고성능 컴퓨팅(HPC) 클러스터에서 **연산 작업을 실제로 수행하는 노드(Worker Node)**를 위한 운영체제 에디션입니다.
- 헤드 노드에서 스케줄링된 작업을 받아 **병렬 처리** 또는 **대량 계산 작업**을 수행하는 역할.

---

## 2. 주요 구성 요소

| 구성 요소     | 설명 |
|--------------|------|
| **Compute Node** | HPC 클러스터에서 실제 계산/처리를 수행하는 노드 |
| **RHEL 기반** | Red Hat Enterprise Linux와 동일한 안정성과 보안 기반 제공 |
| **MPI 지원** | 다수의 노드 간 병렬 연산을 위한 MPI(MPICH, OpenMPI 등) 지원 |
| **Slurm 호환** | Slurm과 같은 작업 스케줄러를 통한 작업 분산 가능 |
| **Self-support** | Red Hat의 기술 지원 없이 사용자 스스로 문제 해결해야 하는 라이선스 유형 |

---

## 3. Self-support란?

| 항목 | 설명 |
|------|------|
| **지원 수준** | Red Hat의 공식 기술 지원 없음 |
| **포함 항목** | 보안 업데이트, 버그 수정, Red Hat Customer Portal 문서 접근 가능 |
| **제외 항목** | 전화, 이메일, 티켓 기반 기술 지원 없음 |
| **적합 대상** | 기술 지원이 불필요하거나 자체 기술 역량이 있는 기업/기관 |

---

## 4. 활용 예시
- 여러 개의 컴퓨트 노드를 클러스터에 연결하여 병렬 작업 수행
- 예) 과학 시뮬레이션, 유전체 분석, 금융 모델링 등에서 사용

---

## 5. 설치 및 구성 요약
1. **헤드 노드에 의해 네트워크 부팅(PXE) 또는 수동 설치**
2. 기본 RHEL 설치 후, 클러스터 구성 요소 설정:
    ```bash
    sudo dnf install openmpi environment-modules
    sudo dnf install slurm slurm-slurmd
    ```
3. 슬럼(Slurm) 슬레이브로 구성:
    - `/etc/slurm/slurm.conf`에서 `NodeName`, `PartitionName` 정의
    - `slurmd` 서비스 시작 및 등록
4. 헤드 노드와 시간 동기화(NTP 또는 Chrony)
5. SSH 키 교환을 통한 비밀번호 없는 접속 설정

---

## 참고 링크
- [Red Hat HPC 설명 페이지](https://www.redhat.com/en/technologies/linux-platforms/enterprise-linux/hpc)
- [Slurm 설치 가이드](https://slurm.schedmd.com/quickstart.html)


### 🎯 Head Node vs Compute Node 역할 차이

| 노드 종류 | 역할 | 특징 |
|-----------|------|------|
| **Head Node (또는 Master Node)** | 클러스터 관리, 작업 스케줄링, 사용자 로그인, 파일 공유, 모니터링 등 | - CPU보다는 안정성과 네트워크 I/O 중요<br>- 24/7 운영됨 |
| **Compute Node** | 실제 연산, 시뮬레이션, 병렬 작업 처리 | - CPU 성능과 메모리 중요<br>- 대량 배치 처리 |
