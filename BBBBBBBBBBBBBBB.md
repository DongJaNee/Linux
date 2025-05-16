# slurm.conf 

SlurmctldPort=6817
SlurmdPort=6818
SlurmdSpoolDir=/var/spool/slurm/slurmd
StateSaveLocation=/var/spool/slurm/slurmctld
SlurmUser=slurm

# 인증 및 로그 설정
AuthType=auth/munge
CryptoType=crypto/munge
SlurmctldLogFile=/var/log/slurm/slurmctld.log
SlurmdLogFile=/var/log/slurm/slurmd.log

# 프로세스 트래킹 설정
ProctrackType=proctrack/cgroup
TaskPlugin=task/none
SelectType=select/cons_res
SelectTypeParameters=CR_Core_Memory

# MPI 설정
MpiDefault=pmi2

# 작업 관리 설정
JobAcctGatherType=jobacct_gather/linux
JobAcctGatherFrequency=30
KillWait=30
WaitTime=30

# 노드 설정 (CPU, 메모리 등은 실제 환경에 맞게 조정)
NodeName=headnode CPUs=40 Sockets=2 CoresPerSocket=10 ThreadsPerCore=2 State=UNKNOWN
NodeName=computenode CPUs=2 Sockets=1 CoresPerSocket=2 ThreadsPerCore=1 State=UNKNOWN

# 파티션 설정
PartitionName=normal Nodes=headnode,computenode Default=YES MaxTime=INFINITE State=UP



