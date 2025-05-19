# SLURM MPI 문제 해결 가이드

## 문제 상황 및 해결 방법

### 1. 노드 상태 문제

**증상:**
```
JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
    8    normal mpi_test  mpiuser PD       0:00      2 (Nodes required for job are DOWN, DRAINED or reserved for jobs in higher priority partitions)
    9    normal mpi_test     root PD       0:00      2 (ReqNodeNotAvail, UnavailableNodes:computenode)
```

**해결 방법:**
1. 컴퓨트 노드 상태 확인
   ```bash
   scontrol show node computenode
   ```

2. 컴퓨트 노드에서 slurmd 서비스 확인 및 재시작
   ```bash
   sudo systemctl status slurmd
   sudo systemctl restart slurmd
   sudo systemctl enable slurmd
   ```

3. 헤드노드와 컴퓨트노드 간 통신 확인
   ```bash
   ping computenode
   ssh computenode
   ```

4. 방화벽 및 SELinux 설정 확인
   ```bash
   sudo systemctl stop firewalld
   sudo setenforce 0
   ```

5. 노드 상태 복구
   ```bash
   scontrol update NodeName=computenode State=RESUME
   ```

6. SLURM 통신 확인
   ```bash
   srun -N1 -n1 -w computenode hostname
   ```

### 2. 실행 파일 누락 문제

**증상:**
```
Job started at 2025. 05. 16. (금) 15:47:20 KST
Running on computenode
slurmstepd: error: execve(): /root/./mpi_hello: No such file or directory
slurmstepd: error: execve(): /root/./mpi_hello: No such file or directory
srun: error: computenode: tasks 0-1: Exited with exit code 2
slurmstepd: error: execve(): /root/./mpi_hello: No such file or directory
srun: error: headnode: tasks 2-3: Exited with exit code 2
Job completed at 2025. 05. 16. (금) 15:47:20 KST
```

**해결 방법:**

#### A. MPI 프로그램 준비

1. MPI Hello World 소스 코드 생성
   ```bash
   cat > mpi_hello.c << 'EOF'
   #include <mpi.h>
   #include <stdio.h>

   int main(int argc, char** argv) {
       MPI_Init(NULL, NULL);
       
       int world_size;
       MPI_Comm_size(MPI_COMM_WORLD, &world_size);
       
       int world_rank;
       MPI_Comm_rank(MPI_COMM_WORLD, &world_rank);

       char processor_name[MPI_MAX_PROCESSOR_NAME];
       int name_len;
       MPI_Get_processor_name(processor_name, &name_len);

       printf("Hello from processor %s, rank %d out of %d processors\n",
              processor_name, world_rank, world_size);

       MPI_Finalize();
       return 0;
   }
   EOF
   ```

2. 컴파일
   ```bash
   module load mpi/openmpi-x86_64
   mpicc -o mpi_hello mpi_hello.c
   ```

#### B. 공유 파일 시스템 설정 (NFS 사용)

1. NFS 공유 디렉토리에 실행 파일 이동
   ```bash
   mkdir -p /home/mpiuser/shared
   cp ~/mpi_hello /home/mpiuser/shared/
   chmod +x /home/mpiuser/shared/mpi_hello
   chown mpiuser:mpiuser /home/mpiuser/shared/mpi_hello
   ```

2. 컴퓨트 노드에서 마운트 확인
   ```bash
   mount -t nfs 192.168.0.44:/home/mpiuser/shared /home/mpiuser/shared
   ls /home/mpiuser/shared/mpi_hello
   ```

3. 공유 디렉토리용 작업 스크립트 생성
   ```bash
   cat > /home/mpiuser/shared/job.slurm << 'EOF'
   #!/bin/bash
   #SBATCH --job-name=mpi_test
   #SBATCH --nodes=2
   #SBATCH --ntasks=4
   #SBATCH --ntasks-per-node=2
   #SBATCH --output=/home/mpiuser/shared/mpi_job_%j.out

   module load mpi/openmpi-x86_64

   echo "Job started at $(date)"
   echo "Running on $(hostname)"

   # 공유 디렉토리 경로로 실행
   srun /home/mpiuser/shared/mpi_hello

   echo "Job completed at $(date)"
   EOF
   ```

4. 권한 설정
   ```bash
   chown mpiuser:mpiuser /home/mpiuser/shared/job.slurm
   ```

5. 작업 제출
   ```bash
   cd /home/mpiuser/shared
   sbatch job.slurm
   ```

#### C. 또는 수동 파일 복사 방법

1. 각 컴퓨트 노드에 실행 파일 복사
   ```bash
   scp mpi_hello root@computenode:/root/
   ```

2. 작업 스크립트 수정
   ```bash
   cat > job.slurm << 'EOF'
   #!/bin/bash
   #SBATCH --job-name=mpi_test
   #SBATCH --nodes=2
   #SBATCH --ntasks=4
   #SBATCH --ntasks-per-node=2
   #SBATCH --output=mpi_job_%j.out

   module load mpi/openmpi-x86_64

   echo "Job started at $(date)"
   echo "Running on $(hostname)"

   # 각 노드의 로컬 경로로 실행
   srun /root/mpi_hello

   echo "Job completed at $(date)"
   EOF
   ```

### 3. 작업 결과 확인

```bash
cat /home/mpiuser/shared/mpi_job_*.out
```

## 참고: Root 권한으로 MPI 실행 시 주의사항

MPI는 root 사용자로 실행을 권장하지 않지만, 필요한 경우 다음 방법으로 실행 가능:

1. `--allow-run-as-root` 옵션 사용
   ```bash
   mpirun --allow-run-as-root -np 4 ./my_program
   ```

2. 환경 변수 설정
   ```bash
   export OMPI_ALLOW_RUN_AS_ROOT=1
   export OMPI_ALLOW_RUN_AS_ROOT_CONFIRM=1
   mpirun -np 4 ./my_program
   ```

3. 또는 비-루트 사용자 생성 및 사용
   ```bash
   adduser mpiuser
   su - mpiuser
   mpirun -np 4 ./my_program
   ```
