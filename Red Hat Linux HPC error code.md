# HPC 구성 중 발생한 주요 오류와 해결 방법

## 1. `slurmctld` 시작 실패 (`Failed with result 'exit-code'`)

* **오류 코드**
```
slurmctld.service: Failed with result 'exit-code'
```

* **원인**
  * `slurm.conf` 내 중복된 항목 또는 필수 설정 누락

* **해결 방법**
```bash
sudo nano /etc/slurm/slurm.conf
```
✅ 중복된 설정 제거 (예: `MpiDefault`, `ProctrackType`, `ReturnToService` 등 한 번만 작성)  
✅ `ControlMachine`, `SlurmctldHost`, `NodeName`, `PartitionName` 등 필수 필드 설정 확인

## 2. `no slurmctldHost defined` / `Unable to process configuration file`

* **오류 코드**
```
fatal: no slurmctldHost defined
```

* **해결 방법**
```bash
# /etc/slurm/slurm.conf 내에 다음과 같은 줄 추가
SlurmctldHost=head-node-hostname
```

## 3. Compute Node 연결 오류 (`ssh`, `scp`, `Permission denied`)

* **오류 코드**
```
Permission denied (publickey,password,keyboard-interactive)
Could not get shadow information for root
```

* **해결 방법**
```bash
# Head Node, Compute Node 모두에서 SELinux 비활성화
sudo setenforce 0

# root 비밀번호 재설정
sudo passwd root

# SSH 설정 허용
sudo nano /etc/ssh/sshd_config
PermitRootLogin yes
PasswordAuthentication yes

# sshd 재시작
sudo systemctl restart sshd
```

## 4. `sinfo` 명령어 오류 (`_parse_next_key: Parsing error`)

* **오류 코드**
```
_parse_next_key: Parsing error at unrecognized key: SlurmPort
```

* **해결 방법**
```bash
# 잘못된 키를 slurm.conf에서 제거
sudo nano /etc/slurm/slurm.conf
# 존재하지 않는 키 (예: SlurmPort, SlurmSpoolDir 등)를 제거
```

## 5. `squeue` 오류 (`UnavailableNodes: localhost`)

* **오류 코드**
```
(ReqNodeNotAvail, UnavailableNodes:localhost)
```

* **해결 방법**
```bash
# Node 상태 재설정
sudo scontrol update NodeName=localhost State=RESUME

# 또는 Node가 드레인된 상태일 때 수동으로 해제
sudo scontrol update NodeName=localhost State=UNDRAIN
```

## 6. `output.txt`가 생성되지 않음

* **오류 코드**
```
cat: output.txt: No such file or directory
```

* **해결 방법**
```bash
# 테스트 스크립트 예시
nano test_job.sh

# 내용
#!/bin/bash
#SBATCH --job-name=test
#SBATCH --output=output.txt
hostname
date

# 제출
sbatch test_job.sh
```

## 7. `Could not get shadow information for root`

* **오류 코드**
```
Could not get shadow information for root
```

* **해결 방법**
```bash
# SELinux 비활성화
sudo setenforce 0
```

## 8. `Invalid node state specified`

* **오류 코드**
```
error: Invalid node state specified
```

* **해결 방법**
```bash
sudo scontrol update NodeName=localhost State=RESUME
```

## 9. Storage 상태 확인 및 Slurm 상태 점검

* **명령어**
```bash
sinfo              # 클러스터 상태 확인
squeue             # 작업 큐 확인
sacct              # 작업 기록 확인
scontrol show nodes # 노드 정보
scontrol show partition
```
