# Chrony 시간 동기화 설정 가이드

## 1. 모든 노드에 `chrony` 설치

```bash
sudo dnf install -y chrony
```

## 2. Head Node를 시간 서버로 설정

### Head Node에서:

```bash
sudo vi /etc/chrony.conf
```

다음 항목을 추가 또는 수정:

```ini
# Listen on local network (예: 192.168.0.0/24)
allow 192.168.0.0/24

# local stratum
local stratum 10

# Chronyd가 서버 역할을 하도록
cmdport 323
bindaddress 0.0.0.0
```

서비스 재시작 및 부팅 시 자동 시작:

```bash
sudo systemctl restart chronyd
sudo systemctl enable chronyd
```

## 3. Compute Node가 Head Node를 시간 서버로 사용하도록 설정

### Compute Node에서:

```bash
sudo vi /etc/chrony.conf
```

기존 `server` 줄을 주석 처리하고 아래 줄 추가:

```ini
server 192.168.0.44 iburst
```

> 참고: 그리고 다른 `server ...` 라인은 모두 주석 처리합니다.

서비스 재시작 및 부팅 시 자동 시작:

```bash
sudo systemctl restart chronyd
sudo systemctl enable chronyd
```

## 4. 시간 동기화 확인

### Compute Node에서 다음 명령 실행:

```bash
chronyc sources -v
```

출력 예시:

```
210 Number of sources = 1
MS Name/IP address         Stratum Poll Reach LastRx Last sample
===============================================================================
^* headnode                     10   6   377    12   -100ns[ -123ns] +/-   15ms
```

> 참고: `^*`는 시간 동기화가 성공했음을 뜻합니다.

## 5. 시간 직접 동기화 테스트 (선택사항)

```bash
sudo chronyc -a 'burst 4/4'
sudo chronyc -a makestep
```
