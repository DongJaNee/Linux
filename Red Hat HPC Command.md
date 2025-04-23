# 💻 HPC 클러스터 구성에 사용된 명령어 및 구성 요소 정리

## 📦 구성 요소

| 명칭/명령어               | 분류            | 설명 |
|--------------------------|----------------|------|
| `slurm`                  | 스케줄러       | HPC 클러스터의 작업을 관리하는 Job Scheduler |
| `slurmctld`              | 데몬           | Slurm Controller Daemon - head-node에서 실행 |
| `slurmd`                 | 데몬           | Slurm Node Daemon - compute-node에서 실행 |
| `munge`                  | 인증 시스템     | Slurm 데몬 간의 인증을 위한 서비스 |
| `sudo`                   | 명령어         | 관리자 권한으로 명령 실행 |
| `ssh`                    | 명령어         | 원격 접속 |
| `scp`                    | 명령어         | 파일을 원격지로 복사 |
| `sinfo`                  | Slurm 명령어    | 클러스터 및 노드 상태 확인 |
| `squeue`                 | Slurm 명령어    | 현재 큐에 등록된 작업 확인 |
| `sbatch`                 | Slurm 명령어    | 작업을 큐에 제출 |
| `sacct`                  | Slurm 명령어    | 완료된 작업 기록 확인 |
| `scontrol`               | Slurm 명령어    | 노드 상태 및 작업 제어 |
| `journalctl`             | 명령어         | 시스템 로그 조회 |
| `setenforce 0`           | 명령어         | SELinux 비활성화 |
| `systemctl`              | 명령어         | 서비스 시작/중지/상태 확인 |
| `chmod`, `chown`         | 명령어         | 파일 권한 및 소유권 변경 |
| `nano`, `vim`            | 편집기         | 설정 파일 수정 |

## 📂 주요 설정/경로

| 경로 또는 파일                 | 설명 |
|-------------------------------|------|
| `/etc/slurm/slurm.conf`       | Slurm 설정 파일 |
| `/etc/munge/munge.key`        | Munge 인증 키 |
| `/var/spool/slurm/`           | 작업 큐 및 로그 저장소 |
| `/var/log/slurmctld.log`      | Controller 로그 |
| `/var/log/slurmd.log`         | Compute 노드 로그 |

## ⚙️ 구성 흐름 요약

### ✅ Head Node
```bash
# 패키지 설치
sudo dnf install slurm munge munge-libs -y

# munge 키 생성
sudo create-munge-key
sudo chown munge: /etc/munge/munge.key
sudo chmod 400 /etc/munge/munge.key

# 서비스 시작
sudo systemctl enable --now munge
sudo systemctl enable --now slurmctld
