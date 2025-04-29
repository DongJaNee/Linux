error code : slurmctld: read_slurm_conf: backup_controller not specified
slurmctld.service: Main process exited, code=exited, status=1/FAILURE
/etc/slurm/slurm.conf 설정 파일이 잘못됐거나, 아예 불완전해.

특히 backup_controller 같은 필수 항목이 없어서 실패했다는 뜻.

2. 예제 slurm.conf 파일 (최소 구성)

# /etc/slurm/slurm.conf 내용 예시

ClusterName=cluster
ControlMachine=localhost

# 기본 포트와 IP
SlurmctldPort=6817
SlurmdPort=6818

# 로그 위치
SlurmctldLogFile=/var/log/slurmctld.log
SlurmdLogFile=/var/log/slurmd.log

# 기본 통신 설정
AuthType=auth/munge
CryptoType=crypto/munge
MpiDefault=none
ProctrackType=proctrack/pgid
ReturnToService=2
SlurmctldPidFile=/var/run/slurmctld.pid
SlurmdPidFile=/var/run/slurmd.pid
StateSaveLocation=/var/spool/slurmctld
SlurmdSpoolDir=/var/spool/slurmd
SwitchType=switch/none
TaskPlugin=task/none

# 노드와 파티션 설정
NodeName=localhost CPUs=2 State=UNKNOWN
PartitionName=debug Nodes=ALL Default=YES MaxTime=INFINITE State=UP

4. 폴더 생성 (없으면 에러남)

sudo mkdir -p /var/spool/slurmctld
sudo mkdir -p /var/spool/slurmd
sudo touch /var/log/slurmctld.log
sudo touch /var/log/slurmd.log
sudo chown slurm:slurm /var/spool/slurmctld /var/spool/slurmd /var/log/slurmctld.log /var/log/slurmd.log

5. Slurm 데몬 재시작

sudo systemctl restart slurmctld
sudo systemctl status slurmctld

💡 만약 hostname 자체를 바꿀 생각이면?

hostname을 예를 들면

    head-node: headnode

    compute-node: computenode

이런 식으로 설정하는 게 더 깔끔해.

hostname 변경 명령어:

sudo hostnamectl set-hostname headnode

변경 후 /etc/hosts 파일에도 추가해야 돼:

sudo nano /etc/hosts

추가:

192.168.0.44 headnode
192.168.0.37 computenode
