# NFS 서버 설정 및 Slurm과 연동 가이드

## 1. Head Node에서 NFS 서버 설정

### 1-1. NFS 설치

```bash
sudo dnf install nfs-utils -y
```

### 1-2. 공유할 디렉터리 생성

```bash
sudo mkdir -p /shared
sudo chown -R nobody:nobody /shared
sudo chmod 777 /shared
```

### 1-3. `/etc/exports` 설정

```bash
echo "/shared 192.168.0.0/24(rw,sync,no_root_squash)" | sudo tee -a /etc/exports
```

> 참고: `192.168.0.0/24`은 Head/Compute Node의 IP 대역으로 조정하세요.

### 1-4. NFS 서비스 시작 및 활성화

```bash
sudo systemctl enable --now nfs-server
```

## 2. Compute Node에서 NFS 클라이언트 설정

### 2-1. NFS 클라이언트 설치

```bash
sudo dnf install nfs-utils -y
```

### 2-2. 마운트 포인트 생성

```bash
sudo mkdir -p /shared
```

### 2-3. Head Node IP 기준으로 마운트

```bash
sudo mount -t nfs 192.168.0.44:/shared /shared
```

> 참고: `192.168.0.44`는 Head Node의 IP 주소입니다.

### 2-4. 정상 작동 확인

```bash
touch /shared/test.txt
```

* Head/Compute Node에서 `ls /shared` 했을 때 같은 내용이 보이면 성공입니다.

## 3. Slurm 실행 준비

### 3-1. MPI 프로그램 준비

```bash
mv mpi_hello.c run_mpi.slurm /shared
cd /shared
mpicc -o mpi_hello mpi_hello.c
```

### 3-2. Slurm 작업 제출

```bash
sbatch run_mpi.slurm
```

### 3-3. 결과 확인

실행이 완료되면 `/shared/slurm-<job_id>.out` 파일이 생성됩니다.
