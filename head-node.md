# 🚀 HPC Head Node 구성 요약 (Red Hat Enterprise Linux 9.5 기준)

## 1. 시스템 업데이트 및 필수 패키지 설치

```bash
su -
dnf update -y
dnf groupinstall "Development Tools" -y
```

## 2. Munge 설치 및 활성화
```bash
dnf install -y munge munge-libs
/usr/sbin/create-munge-key
systemctl enable --now munge
```

## 3. EPEL 및 Slurm 저장소 추가
```bash
subscription-manager repos --enable codeready-builder-for-rhel-9-$(uname -m)-rpms
dnf install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm
dnf clean all
dnf makecache
curl -o /etc/yum.repos.d/slurm.repo https://download.schedmd.com/slurm/slurm.repo
```

## 4. Slurm 패키지 설치
```bash
dnf install -y slurm slurm-slurmd slurm-slurmctld
```

## 5. Slurm 설정 파일 생성 및 편집
```bash
mkdir -p /etc/slurm
curl -o /etc/slurm/slurm.conf https://raw.githubusercontent.com/SchedMD/slurm/master/etc/slurm.conf.example
nano /etc/slurm/slurm.conf

ControlMachine=localhost.localdomain
NodeName=localhost.localdomain CPUs=2 Sockets=1 CoresPerSocket=2 ThreadsPerCore=1 State=UNKNOWN
PartitionName=debug Nodes=localhost.localdomain Default=YES MaxTime=INFINITE State=UP
```

## 6. Slurm 서비스 시작
```bash
systemctl enable --now slurmctld
systemctl enable --now slurmd
```

## 7. 상태 확인
```bash
sinfo
```
