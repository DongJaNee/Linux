# Slurm HPC 오류 및 해결 방법

## 1. `Failed with result 'exit-code'`

**오류 코드**: 
```
slurmctld.service: Failed with result 'exit-code'
```

**원인**: 
- `slurm.conf` 내 중복 설정 또는 필수 설정 누락

**해결 방법**:
- `slurm.conf`에서 중복 키 제거
- `ControlMachine`, `NodeName`, `PartitionName` 확인

## 2. `no slurmctldHost defined`

**오류 코드**: 
```
fatal: unable to process configuration file
```

**원인**: 
- `slurm.conf`에서 `SlurmctldHost` 또는 `ControlMachine` 누락

**해결 방법**:
```
ControlMachine=head-node-hostname
```

## 3. `Could not get shadow information for root`

**오류 코드**: 
```
sshd[PID]: error: Could not get shadow information for root
```

**원인**: 
- SELinux 정책으로 인해 SSH 로그인 실패

**해결 방법**:
```bash
setenforce 0
```

## 4. `Permission denied (publickey,password,keyboard-interactive)`

**원인**: 
- SSH 설정 미비, 비밀번호 인증 불가

**해결 방법**:
```bash
# ssh 설정 수정
nano /etc/ssh/sshd_config

# 다음 항목 확인 또는 수정
PermitRootLogin yes
PasswordAuthentication yes

# sshd 재시작
systemctl restart sshd
```

## 5. `Duplicated NodeHostName localhost`

**오류 코드**: 
```
error: Duplicated NodeHostName localhost in config file
```

**원인**: 
- `slurm.conf`에 중복된 Node 설정

**해결 방법**:
- `NodeName=localhost` 항목이 하나만 존재하도록 수정

## 6. `ReqNodeNotAvail, UnavailableNodes:localhost`

**오류 코드**:
```
NODELIST(REASON) localhost (ReqNodeNotAvail, UnavailableNodes:localhost)
```

**원인**: 
- compute node가 slurmctld에 연결되지 않음

**해결 방법**:
```bash
# slurmd 재시작
systemctl restart slurmd

# slurmctld에서 상태 갱신
scontrol update NodeName=localhost State=RESUME
```

## 7. `Parsing error at unrecognized key`

**오류 코드**: 
```
_parse_next_key: Parsing error at unrecognized key: <keyname>
```

**원인**: 
- `slurm.conf`에 오타 또는 잘못된 키 존재

**해결 방법**:
- `SlurmPort`, `SlurmSpoolDir` 등 유효한 키인지 공식 문서 참고

## 8. `slurm_load_partitions: Unable to contact slurm controller`

**오류 코드**: 
```
slurm_load_partitions: Unable to contact slurm controller (connect failure)
```

**원인**: 
- slurmctld가 실행 중이지 않음 또는 포트 차단

**해결 방법**:
```bash
# slurmctld 실행
systemctl start slurmctld
```
