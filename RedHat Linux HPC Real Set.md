# Slurm 클러스터 구축 가이드 (RHEL 9.5 기준)

## 📌 환경 구성
* Head Node IP: `192.168.0.44`, hostname: `headnode`
* Compute Node IP: `192.168.0.37`, hostname: `computenode`

## 🔹 Head Node 작업

### 1. 시스템 준비
```bash
sudo subscription-manager repos --enable codeready-builder-for-rhel-9-$(uname -m)-rpms (epel download)
sudo dnf install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm (epel download)

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


sudo nano /etc/ssh/sshd_config 
PermitRootLogin yes (# 제거)
PasswordAuthentication yes (# 제거)

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

Slurm 사용자 및 그룹 생성
```bash
sudo useradd -r -c "Slurm user" -s /sbin/nologin slurm
sudo chown slurm:slurm /var/log/slurmctld.log
```

Slurm 디렉토리 준비
```bash
sudo mkdir -p /var/spool/slurmctld
sudo chown slurm:slurm /var/spool/slurmctld
sudo touch /var/log/slurmctld.log
sudo chown slurm:slurm /var/log/slurmctld.log
```
### 7. Slurmctld Daemon 설정 
```bash
sudo vi /etc/systemd/system/slurmctld.service
```
```bash
[Unit]
Description=Slurm controller daemon
After=network.target munge.service

[Service]
Type=simple
User=slurm
ExecStart=/usr/sbin/slurmctld -D
Restart=always

[Install]
WantedBy=multi-user.target
```

### 8. Slurmd Daemon 설정 
```bash
sudo vi /etc/systemd/system/slurmd.service
```
```bash
[Unit]
Description=Slurm node daemon
After=network.target munge.service

[Service]
Type=simple
User=root
ExecStart=/usr/sbin/slurmd -D
Restart=always

[Install]
WantedBy=multi-user.target
```
```적용 및 실행
sudo systemctl daemon-reload
sudo systemctl enable --now slurmctld   # Head node만
sudo systemctl enable --now slurmd      # 모든 노드에서
```
```상태 확인
systemctl status slurmctld
systemctl status slurmd
```

### 9. Slurm 서비스 실행
```bash
sudo systemctl enable --now slurmctld
```

## 🔹 Compute Node 작업

### 1. 시스템 준비
```bash
sudo subscription-manager repos --enable codeready-builder-for-rhel-9-$(uname -m)-rpms (epel download)
sudo dnf install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm (epel download)

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

sudo firewall-cmd --add-service=ssh --permanent (방화벽 열기)
sudo firewall-cmd --reload (방화벽 열기)
```

```bash
sudo nano /etc/ssh/sshd_config 
PermitRootLogin yes (# 제거)
PasswordAuthentication yes (# 제거)

sudo systemctl restart sshd (sshd 데몬 재시작)

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
sudo groupadd slurm (slurm 그룹생성)
sudo useradd -g slurm slurm (slurm 해당그룹에 추가 생성)  



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
#SBATCH --job-name=gpu_test
#SBATCH --output=/home/slurm/gpu_result.txt
#SBATCH --error=/home/slurm/gpu_error.txt
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=1
#SBATCH --mem=1G
#SBATCH --time=00:02:00
#SBATCH --partition=debug
#SBATCH --gres=gpu:1  # GPU 1개 요청 (Slurm에 GRES 설정이 있어야 함)

echo "=== GPU 테스트 시작 ==="
hostname
date
echo ""
echo "=== GPU 정보 ==="
nvidia-smi

```

```bash
sbatch test_job.sh
cat result.txt
```

**※Cat 명령어가 실행이 안되거나 hostname 이 다르면 hostname을 맞추고 sudo reboot로 서버 reboot시키고 munge와 slurm 재실행**
## ✅ 참고
* `scontrol ping` 으로 노드 연결 상태 확인 가능
* 모든 설정 후 방화벽, SELinux가 차단하고 있는지 여부 확인 필
