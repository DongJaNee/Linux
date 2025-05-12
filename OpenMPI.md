# OpenMPI 클러스터 설정 및 Slurm 통합 가이드

## 1. OpenMPI 설치 (Head 및 Compute Node 공통)

모든 노드에 동일하게 설치해야 합니다. 다음은 RHEL 계열 기준입니다.

```bash
sudo dnf install openmpi openmpi-devel -y
```

설치 후 환경 변수 설정:

```bash
echo "export PATH=/usr/lib64/openmpi/bin:\$PATH" >> ~/.bashrc
echo "export LD_LIBRARY_PATH=/usr/lib64/openmpi/lib:\$LD_LIBRARY_PATH" >> ~/.bashrc
source ~/.bashrc
```

> 참고: 위 경로는 `dnf`로 설치했을 때의 기본 경로이며, 직접 빌드한 경우 경로가 다를 수 있습니다.

## 2. MPI 프로그램 Slurm에서 실행하기

### MPI 예제 코드 (`mpi_hello.c`):

```c
#include <mpi.h>
#include <stdio.h>

int main(int argc, char** argv) {
    MPI_Init(NULL, NULL);
    
    int world_rank, world_size;
    MPI_Comm_rank(MPI_COMM_WORLD, &world_rank);
    MPI_Comm_size(MPI_COMM_WORLD, &world_size);
    
    printf("Hello from rank %d of %d\n", world_rank, world_size);
    
    MPI_Finalize();
    return 0;
}
```

### 컴파일:

```bash
mpicc -o mpi_hello mpi_hello.c
```

### Slurm 작업 스크립트 (`run_mpi.slurm`):

```bash
#!/bin/bash
#SBATCH --job-name=mpi_test
#SBATCH --ntasks=4
#SBATCH --nodes=2
#SBATCH --time=00:05:00

srun ./mpi_hello
```

### 작업 제출:

```bash
sbatch run_mpi.slurm
```
