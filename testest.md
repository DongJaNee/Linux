# Slurm 클러스터 구축 가이드 (RHEL 9.5 기준)

## 📌 환경 구성
* Head Node IP: `192.168.0.44`, hostname: `headnode`
* Compute Node IP: `192.168.0.37`, hostname: `computenode`

## 🔹 Head Node 작업

### 1. 시스템 준비
```bash
sudo dnf install -y epel-release
sudo dnf install -y munge munge-libs munge-devel slurm slurm-slurmd slurm-slurmctld
```

### 2. 호스트명 및 hosts 파일 설정
```bash
sudo hostnamectl set-hostname headnode
```

`/etc/hosts` 파일에 다음 내용 추가:
```
192.168.0.44 headnode
192.168.0.37 computenode
```

### 3. SSH 설정
```bash
sudo systemctl enable sshd --now
```

### 4. Munge 설정
```bash
sudo /usr/sbin/create-munge-key
sudo chown munge:munge /etc/munge/munge.key
sudo chmod 400 /etc/munge/munge.key
sudo systemctl enable --now munge
```

### 5. Slurm 설정

`slurm.conf` 생성
```bash
sudo vi /etc/slurm/slurm.conf
```

예시:
```
ClusterName=cluster
ControlMachine=headnode
SlurmUser=slurm
SlurmctldPort=6817
SlurmdPort=6818
AuthType=auth/munge
StateSaveLocation=/var/spool/slurmctld
SlurmdSpoolDir=/var/spool/slurmd
SwitchType=switch/none
MpiDefault=none
SlurmctldPidFile=/var/run/slurmctld.pid
SlurmdPidFile=/var/run/slurmd.pid
ProctrackType=proctrack/pgid
ReturnToService=2
SlurmctldTimeout=120
SlurmdTimeout=300
SchedulerType=sched/backfill
SelectType=select/cons_res
SelectTypeParameters=CR_Core
NodeName=computenode NodeAddr=192.168.0.37 CPUs=2 State=UNKNOWN
PartitionName=debug Nodes=computenode Default=YES MaxTime=INFINITE State=UP
```

Slurm 디렉토리 준비
```bash
sudo mkdir -p /var/spool/slurmctld
sudo chown slurm:slurm /var/spool/slurmctld
sudo touch /var/log/slurmctld.log
sudo chown slurm:slurm /var/log/slurmctld.log
```

### 6. Slurm 서비스 실행
```bash
sudo systemctl enable --now slurmctld
```

## 🔹 Compute Node 작업

### 1. 시스템 준비
```bash
sudo dnf install -y epel-release
sudo dnf install -y munge munge-libs munge-devel slurm slurm-slurmd
```

### 2. 호스트명 및 hosts 파일 설정
```bash
sudo hostnamectl set-hostname computenode
```

`/etc/hosts` 파일에 다음 내용 추가:
```
192.168.0.44 headnode
192.168.0.37 computenode
```

### 3. SSH 설정
```bash
sudo systemctl enable sshd --now
```

### 4. Munge 키 복사 및 설정
```bash
# Head Node에서 실행:
scp /etc/munge/munge.key root@192.168.0.37:/etc/munge/munge.key

# Compute Node에서 실행:
sudo chown munge:munge /etc/munge/munge.key
sudo chmod 400 /etc/munge/munge.key
sudo systemctl enable --now munge
```

### 5. slurm.conf 복사
```bash
# Head Node에서 실행:
scp /etc/slurm/slurm.conf root@192.168.0.37:/etc/slurm/slurm.conf
```

### 6. Slurm 디렉토리 준비
```bash
sudo mkdir -p /var/spool/slurmd
sudo chown slurm:slurm /var/spool/slurmd
sudo touch /var/log/slurmd.log
sudo chown slurm:slurm /var/log/slurmd.log
```

### 7. (필요 시) slurmd 서비스 수동 생성
경로 확인 후 아래처럼 직접 생성
```bash
sudo vi /etc/systemd/system/slurmd.service
```

```
[Unit]
Description=Slurm node daemon
After=network.target munge.service

[Service]
Type=simple
ExecStart=/usr/sbin/slurmd -D
User=root

[Install]
WantedBy=multi-user.target
```

```bash
sudo systemctl daemon-reexec
```

### 8. slurmd 서비스 시작
```bash
sudo systemctl enable --now slurmd
```

## 🔍 상태 확인 및 테스트

Head Node에서 확인
```bash
sinfo
scontrol show nodes
```

Slurm 테스트 스크립트 (test_job.sh)
```bash
#!/bin/bash
#SBATCH --job-name=testjob
#SBATCH --output=result.txt
#SBATCH --ntasks=1

hostname
```

```bash
sbatch test_job.sh
cat result.txt
```

## ✅ 참고
* `scontrol ping` 으로 노드 연결 상태 확인 가능
* 모든 설정 후 방화벽, SELinux가 차단하고 있는지 여부 확인 필
