# Red Hat Linux HPC 설정 가이드

이 가이드는 Red Hat Linux에서 HPC(High-Performance Computing) 환경을 설정하고 발생 가능한 오류를 해결하는 방법을 제공합니다.

## Head Node 설정

### 1. 시스템 업데이트 및 패키지 설치

```bash
# root 계정으로 접속
su -

# 시스템 업데이트
sudo dnf update -y

# 개발 도구 설치
sudo dnf groupinstall "Development Tools" -y
sudo subscription-manager register --username [당신의 Red Hat ID] --password [비밀번호]
sudo subscription-manager attach --auto
sudo dnf install openmpi openmpi-devel environment-modules 

# Munge 설치 및 설정
sudo dnf install -y munge munge-libs
sudo /usr/sbin/create-munge-key
sudo systemctl enable munge --now

# EPEL 저장소 설치
sudo dnf install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm
sudo dnf clean all
sudo dnf makecache  # 저장소 캐시 갱신

# Slurm 저장소 추가
sudo curl -o /etc/yum.repos.d/slurm.repo https://download.schedmd.com/slurm/slurm.repo
```

### 2. SSH 설정

```bash
sudo nano /etc/ssh/sshd_config
```

다음 설정으로 변경:
```
PermitRootLogin yes
PasswordAuthentication yes
ChallengeResponseAuthentication no
UsePAM yes
```

```bash
# SSH 서비스 재시작
sudo systemctl restart sshd

# 방화벽에서 SSH 허용
sudo firewall-cmd --add-service=ssh --permanent
sudo firewall-cmd --reload

# SELinux 설정 변경 (필요시)
sudo setenforce 0
```

### 3. Slurm 컨트롤러 설치 및 설정

```bash
# Slurm 패키지 설치
sudo dnf install -y slurm slurm-slurmd slurm-slurmctld munge munge-libs

# 설정 파일 복사
sudo cp /etc/slurm/slurm.conf.example /etc/slurm/slurm.conf
```

### 4. Slurm 설정 파일 수정

```bash
sudo nano /etc/slurm/slurm.conf
```

기본 설정 예시:
```
NodeName=localhost CPUs=2 Sockets=1 CoresPerSocket=2 ThreadsPerCore=1 State=UNKNOWN
PartitionName=debug Nodes=localhost Default=YES MaxTime=INFINITE State=UP
```

### 5. Slurm 서비스 시작

```bash
sudo systemctl start slurmctld
sudo systemctl enable slurmctld
sudo systemctl status slurmctld
```

### 6. 설치된 패키지 확인

```bash
# Slurm 패키지 확인
dnf list installed | grep slurm
# 확인: slurm, slurm-devel, slurm-libs, slurm-slurmctld, slurm-slurmd

# Munge 패키지 확인
dnf list installed | grep munge
# 확인: munge, munge-libs
```

### 7. Compute Node로 munge 키 복사

```bash
scp /etc/munge/munge.key root@<compute-node-ip>:/etc/munge/
```

### 8. Compute Node와 연결 확인

```bash
sinfo
scontrol show nodes
```

## Compute Node 설정

### 1. 시스템 업데이트 및 패키지 설치

```bash
# root 계정으로 접속
su -

# 시스템 업데이트
sudo dnf update -y

# 개발 도구 설치
sudo dnf groupinstall "Development Tools" -y
sudo subscription-manager attach --auto

# EPEL 저장소 설치
sudo dnf install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm
sudo dnf clean all
sudo dnf makecache  # 저장소 캐시 갱신

# Slurm 저장소 추가
sudo curl -o /etc/yum.repos.d/slurm.repo https://download.schedmd.com/slurm/slurm.repo

# mpi 설치
sudo dnf install openmpi environment-modules
```

### 2. SSH 설정

```bash
sudo nano /etc/ssh/sshd_config
```

다음 설정으로 변경:
```
PermitRootLogin yes
PasswordAuthentication yes
ChallengeResponseAuthentication no
UsePAM yes
```

```bash
# SSH 서비스 재시작
sudo systemctl restart sshd

# 방화벽에서 SSH 허용
sudo firewall-cmd --add-service=ssh --permanent
sudo firewall-cmd --reload

# SELinux 설정 변경 (필요시)
sudo setenforce 0
```

### 3. 필수 패키지 설치

```bash
# 최소 필수 패키지 설치
sudo dnf install -y slurm slurm-devel munge munge-libs
```

### 4. Head Node에서 복사한 Munge 키 설정

```bash
# 이미 Head Node에서 복사했다고 가정
sudo chown -R munge:munge /etc/munge
sudo chmod 400 /etc/munge/munge.key
sudo systemctl enable --now munge
```

### 5. Slurm 디렉토리 생성 및 권한 설정

```bash
sudo mkdir -p /var/spool/slurm
sudo mkdir -p /var/log
sudo touch /var/log/slurmd.log
sudo chown -R slurm: /var/spool/slurm /var/log/slurmd.log
```

### 6. Slurm 설정 파일 복사

```bash
# Head Node에서 설정 파일 복사
scp root@<head-node-ip>:/etc/slurm/slurm.conf /etc/slurm/
```
### 7. Slurm 클러스터 설정 가이드: Head Node와 Compute Node 간 연결 구성
```bash
이 가이드는 Slurm이 Head Node와 Compute Node 간에 정상적으로 동작하도록 설정하는 방법을 단계별로 설명합니다.

## 1. Hostname 설정 및 확인

각 노드에서 호스트명을 적절히 설정합니다:

# Head node에서 실행
hostnamectl set-hostname headnode

# Compute node에서 실행
hostnamectl set-hostname computenode

## 2. /etc/hosts 설정

모든 노드에서 동일하게 `/etc/hosts` 파일을 설정합니다:

127.0.0.1 localhost localhost.localdomain
192.168.0.44 headnode
192.168.0.37 computenode

> **참고**: `192.168.0.44`, `192.168.0.37`은 예시 IP이므로 실제 환경에 맞는 IP 주소로 변경하세요.

## 3. slurm.conf 설정

Head node에서 `/etc/slurm/slurm.conf` 파일을 다음과 같이 설정합니다:

ControlMachine=headnode
NodeName=headnode CPUs=4 RealMemory=4096 State=UNKNOWN
NodeName=computenode CPUs=4 RealMemory=4096 State=UNKNOWN
PartitionName=debug Nodes=ALL Default=YES MaxTime=INFINITE State=UP

설정 후 compute node로 복사합니다:

scp /etc/slurm/slurm.conf root@computenode:/etc/slurm/

## 4. Slurm 서비스 수동 생성 (필요 시)

일반적으로 패키지 설치 시 자동 생성되지만, 없을 경우 수동으로 생성합니다.

### slurmctld.service (Head node용)

`/usr/lib/systemd/system/slurmctld.service` 파일 생성:

[Unit]
Description=Slurm controller daemon
After=network.target munge.service
Requires=munge.service

[Service]
Type=simple
ExecStart=/usr/sbin/slurmctld -D
ExecReload=/bin/kill -HUP $MAINPID
KillMode=process
Restart=always

[Install]
WantedBy=multi-user.target

### slurmd.service (Compute node용)

`/usr/lib/systemd/system/slurmd.service` 파일도 비슷하게 생성합니다.

## 5. 서비스 재시작

### Head node:

sudo systemctl daemon-reexec
sudo systemctl restart munge
sudo systemctl restart slurmctld

### Compute node:

sudo systemctl daemon-reexec
sudo systemctl restart munge
sudo systemctl restart slurmd

## 6. 설정 확인

### 모든 노드에서 다음 서비스 상태 확인:

systemctl status munge
systemctl status slurmctld  # Head node에서만
systemctl status slurmd

### Head node에서 클러스터 상태 확인:

sinfo  # 노드 목록과 상태 확인

정상적으로 설정되었다면 모든 노드가 클러스터에 등록되고 사용 가능한 상태로 표시됩니다.
```
### 7. Slurm 서비스 시작

```bash
sudo systemctl start slurmd
sudo systemctl enable slurmd
sudo systemctl status slurmd
computenode Slurm 서비스 오류  

compute node 에서 slurm이 시작이 되지 않을 경우 
->slurmd의 service 파일 생성이 안됨.
해결 : sudo nano /etc/systemd/system/slurmd.service 
[Unit]
Description=Slurm Daemon
After=network.target

[Service]
ExecStart=/usr/sbin/slurmd
Type=simple
Restart=always

[Install]
WantedBy=multi-user.target

```

## 작업 제출 및 확인 (Head Node에서)

### 1. 테스트 작업 스크립트 생성

```bash
nano test_job.sh
```

```bash
#!/bin/bash
#SBATCH --job-name=test
#SBATCH --output=output.txt
#SBATCH --time=00:01:00
#SBATCH --ntasks=1

echo "===== 기본 시스템 정보 ====="
hostname
whoami
date
uptime

echo "===== CPU 정보 ====="
lscpu

echo "===== 메모리 정보 ====="
free -h

echo "===== 디스크 정보 ====="
df -h

echo "===== 환경 변수 ====="
env

echo "===== 현재 작업 디렉토리 ====="
pwd

echo "===== 네트워크 인터페이스 ====="
ip addr show

echo "===== Slurm 관련 정보 ====="
scontrol show job $SLURM_JOB_ID
```

### 2. 작업 제출 및 확인

```bash
# 작업 제출
sbatch test_job.sh

# 작업 상태 확인
squeue

# 결과 확인
cat output.txt
```

## 오류 해결 가이드

### 패키지 설치 관련 오류

**오류**: "일치하는 인수가 없습니다: munge-devel, slurm"

**해결방법**:
```bash
# EPEL 저장소 수동 설치 확인
sudo dnf install -y epel-release

# 저장소 목록 확인
dnf repolist

# 최소 필수 패키지 설치
sudo dnf install -y slurm slurm-devel munge munge-libs
```

### SSH 연결 오류

**오류**: "ssh: connect to host 192.168.0.xx port 22: Connection refused"

**해결방법**:
- SSH 설정 확인
- 방화벽 설정 확인
- SSH 서비스 재시작

### Munge 키 관련 오류

**오류**: "stat local '/etc/munge/munge.key': No such file or directory"

**해결방법**:
```bash
# Munge 키 생성 여부 확인
ls -l /etc/munge/munge.key

# 키가 없다면 생성
sudo /usr/sbin/create-munge-key

# 키 권한 설정
sudo chmod 400 /etc/munge/munge.key
sudo chown munge:munge /etc/munge/munge.key
```

### Munge 인증 오류

**오류**: Munge 인증 실패

**해결방법**:
- Head Node와 Compute Node의 munge.key가 동일한지 확인
- 키 권한 설정이 올바른지 확인
- Munge 서비스 재시작

### Slurm 데몬 시작 오류

**오류**: Slurm 데몬이 시작되지 않음

**해결방법**:
```bash
# 로그 확인
sudo journalctl -u slurmctld  # Head Node
sudo journalctl -u slurmd     # Compute Node

# 디렉토리 권한 확인
sudo ls -la /var/spool/slurm
sudo ls -la /var/log/slurmd.log

# 필요시 권한 재설정
sudo chown -R slurm: /var/spool/slurm /var/log/slurmd.log
```

### 노드 상태 오류

**오류**: 노드가 DOWN 상태로 표시됨

**해결방법**:
```bash
# 노드 상태 확인
sinfo

# 노드 수동 복구 시도
sudo scontrol update nodename=<node-name> state=resume

# 설정 파일 확인
sudo cat /etc/slurm/slurm.conf
```
