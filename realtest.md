# Slurm 클러스터 설정 가이드: Head Node와 Compute Node 간 연결 구성

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
