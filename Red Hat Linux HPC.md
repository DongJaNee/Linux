# Slurm HPC 환경 설정 가이드

이 가이드는 Head Node와 Compute Node를 설정하여 Slurm 클러스터를 구축하는 방법을 다룹니다. 각 단계별로 발생할 수 있는 오류를 피하기 위한 조언도 포함되어 있습니다.

## 1. Head Node 설정

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

### password 활성화
```shell
sudo setenforce 0
```

### 2.1. Munge 키 복사
```shell
sudo scp /etc/munge/munge.key root@<compute-node-ip>:/etc/munge/
```

### 2.2. Slurm Daemon 설치
```shell
sudo dnf install -y slurm slurm-devel slurm-munge slurm-plugins
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

### 4.1. 테스트 작업 제출
```shell
sbatch test_job.sh
```

### 4.2. 배치 작업 스크립트 (nano test_job.sh)
```bash
#!/bin/bash
#SBATCH --job-name=test  
#SBATCH --output=output.txt  
#SBATCH --time=00:10:00  
echo "Hello from compute node"  
hostname
```

### 4.3. 작업 상태 확인
```shell
squeue
```

### 4.4. 결과 확인
```shell
cat output.txt
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

## 6. 결론

이 가이드를 따라 설정한 후, Head Node와 Compute Node가 성공적으로 연결되고, Slurm을 통해 작업을 스케줄링하여 실행할 수 있습니다. 모든 오류는 journalctl 명령어로 로그를 확인하고 slurm.conf 파일을 점검하는 방식으로 해결할 수 있습니다.
