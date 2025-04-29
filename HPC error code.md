## error 2300 
- "slurmd가 자신의 NodeName을 slurmctld 서버에 등록할 때 실패했다"
보통 NodeName 문제나 hostname 설정 문제 때문에 발생합니다.

### 해결 방법 
#### 1. Compute Node의 hostname이 정확한지 확인
```bash
hostname (slurm.conf의 NodeName과 hostname 명령어 결과가 정확히 일치해야 합니다.)
ex) slurm.conf → NodeName=computenode
    hostname → computenode
    /etc/hosts → 192.168.0.37 computenode

sudo hostnamectl set-hostname computenode (hostname이 이상하면 수정)
```

#### 2. /etc/hosts 파일 수정 확인
```bash
sudo nano /etc/hosts

ex) 127.0.0.1 localhost
    192.168.0.44 headnode
    192.168.0.37 computenode
```

#### 3. slurm.conf 파일 점검
- compute node와 head node 둘 다 slurm.conf에 NodeName=computenode가 들어가 있어야 합니다.
- ControlMachine은 headnode 이름이어야 합니다.

```bash
ControlMachine=headnode
NodeName=computenode State=UNKNOWN
PartitionName=debug Nodes=computenode Default=YES MaxTime=INFINITE State=UP
```

#### 4. 서비스 재시작 (양쪽 다)
```bash
sudo systemctl restart slurmctld
sudo systemctl restart slurmd
```

--- 



## 1. head node에서 slurmctld 상태 점검
```bash
sudo systemctl status slurmctld
```
- 죽어있던 slurmdctld를 살린다.

## 2. compute node에서 slurmd 재시작 
```bash
sudo systemctl restart slurmd
```

## 3. 네트워크 점검 (ping으로 headnode 통신 되는지 확인)
```bash
ping <headnode-IP>
```

## 4. IP 고정 여부 확인 (DHCP라면 static IP 설정 권장)

## 5. 필요 시 slurmd 로그 확인
```bash
sudo journalctl -u slurmd
```


