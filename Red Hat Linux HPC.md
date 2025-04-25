# Red Hat Linux HPC 설정 

## 1. Head Node 설정

### 시스템 업데이트 및 개발 도구 설치

```bash
su - (root관리자로 접속)
sudo dnf update -y
sudo dnf groupinstall "Development Tools" -y


sudo dnf install -y munge munge-libs
sudo /usr/sbin/create-munge-key
sudo systemctl enable munge --now

sudo dnf install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm (EPEL Repository 수동설치)
sudo dnf clean all
sudo dnf makecache (저장소 캐시 갱신)
sudo curl -o /etc/yum.repos.d/slurm.repo https://download.schedmd.com/slurm/slurm.repo (slurm 저장소 추가)
```

### 1.1. 시스템 업데이트
```shell
sudo dnf update -y
```

### 1.2. Munge 서비스 시작
```shell
sudo systemctl start munge
sudo systemctl enable munge
```

### 1.3. Slurm 컨트롤러 설정
```shell
sudo dnf install -y slurm slurm-slurmd slurm-slurmctld munge munge-libs
sudo cp /etc/slurm/slurm.conf.example /etc/slurm/slurm.conf
```

### 1.4. Slurm 컨트롤러 서비스 시작
```shell
sudo systemctl start slurmctld
sudo systemctl enable slurmctld
```

### 1.5. Slurm 상태 확인
```shell
sudo systemctl status slurmctld
```

## 2. Compute Node 설정

### 시스템 업데이트 및 개발 도구 설치

```bash
su - (root관리자로 접속)
sudo dnf update -y
sudo dnf groupinstall "Development Tools" -y


sudo dnf install -y munge munge-libs
sudo /usr/sbin/create-munge-key
sudo systemctl enable munge --now

sudo dnf install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm (EPEL Repository 수동설치)
sudo dnf clean all
sudo dnf makecache (저장소 캐시 갱신)
sudo curl -o /etc/yum.repos.d/slurm.repo https://download.schedmd.com/slurm/slurm.repo (slurm 저장소 추가)
```


### head node config 설정 
```shell
sudo nano /etc/ssh/sshd_config

PermitRootLogin yes
PasswordAuthentication yes
ChallengeResponseAuthentication no
UsePAM yes
```
### password 활성화
```shell
sudo setenforce 0
```
sudo dnf install -y slurm slurm-devel munge munge-libs

### Munge 권한 및 서비스 설정 
```shell
sudo chown -R munge:munge /etc/munge
sudo chmod 400 /etc/munge/munge.key
sudo systemctl enable --now munge
```

### Slurm 디렉토리 생성 및 권한 설정 
```shell
sudo mkdir -p /var/spool/slurm
sudo mkdir -p /var/log
sudo touch /var/log/slurmd.log

sudo chown -R slurm: /var/spool/slurm /var/log/slurmd.log
```

### 2.1. Munge 키 복사
```shell
sudo scp /etc/munge/munge.key root@<compute-node-ip>:/etc/munge/
```

### 2.3. Slurm 서비스 시작
```shell
sudo systemctl start slurmd
sudo systemctl enable slurmd
```

### 2.4. Compute Node 상태 확인
```shell
sudo systemctl status slurmd
```

## 3. Head Node와 Compute Node 연결

### 3.1. Slurm 노드 설정
slurm.conf에서 노드 정보를 설정합니다. Head Node와 Compute Node에서 동일한 노드 이름을 사용해야 합니다. 예시:

```
NodeName=localhost CPUs=2 Sockets=1 CoresPerSocket=2 ThreadsPerCore State=UNKNOWN
PartitionName=debug Nodes=localhost Default=YES MaxTime=INFINITE State=UP
```

### 3.2. Slurm 상태 확인
```shell
sinfo
```

Compute Node가 `UP` 상태여야 합니다.

## 4. 작업 제출 및 상태 확인

### 4.1. 배치 작업 스크립트 (nano test_job.sh)
```shell
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

### 4.2. 테스트 작업 제출
```shell
sbatch test_job.sh
```

### 4.3. 작업 상태 확인
```shell
squeue
```

### 4.4. 결과 확인
```shell
cat output.txt
```

### 4.5 nano  test_job.sh에서 수정을 한 뒤 
```
sbatch test_job.sh에 제출을 하고난 뒤 
cat output.txt 실행 
```

## 5. 문제 해결

### 5.1. Munge 서비스 오류
Munge 서비스가 제대로 실행되지 않으면 다음 명령어로 확인하고 재시작할 수 있습니다.
```shell
sudo systemctl status munge  
sudo systemctl restart munge
```

### 5.2. Slurmctld 서비스 오류
Slurm 컨트롤러가 실행되지 않으면 로그를 확인합니다.
```shell
sudo journalctl -u slurmctld
```
로그에서 error 메시지를 확인하고 slurm.conf 파일을 다시 점검하여 수정합니다.

### 5.3. Slurm 상태 오류
slurmd 서비스가 실행되지 않거나 scontrol에서 오류가 발생하면, slurmd 서비스 로그를 확인합니다.
```shell
sudo journalctl -u slurmd
```

### 5.4. SSH 연결 오류
ssh 연결이 안될 경우, sshd_config에서 PermitRootLogin yes와 PasswordAuthentication yes가 설정되어 있는지 확인합니다. 이후 SSH 서비스를 재시작합니다.
```shell
sudo systemctl restart sshd
```

### 5.5 sudo dnf install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm status 404 오류
1. EPEL설치
```
sudo dnf install -y epel-release
```
2. .rpm 파일 직접 다운로드
```
sudo dnf install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm
```

## 6. 결론

이 가이드를 따라 설정한 후, Head Node와 Compute Node가 성공적으로 연결되고, Slurm을 통해 작업을 스케줄링하여 실행할 수 있습니다. 모든 오류는 journalctl 명령어로 로그를 확인하고 slurm.conf 파일을 점검하는 방식으로 해결할 수 있습니다.
