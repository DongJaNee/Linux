마스터 노드(Headnode) 설정
1. 사전 확인 작업

bash
# 컴퓨트 노드 연결 테스트
ssh compute1 hostname

2. OpenMPI 설치 및 종속성 확인

bash

# 필요한 패키지 설치
sudo dnf clean all
sudo dnf makecache

# 개발 도구 설치 확인 및 설치
sudo dnf groupinstall "Development Tools" -y

# OpenMPI 및 관련 패키지 설치
sudo dnf install -y \
  environment-modules \
  gcc-gfortran \
  openmpi openmpi-devel \
  hwloc hwloc-devel \
  numactl numactl-devel

# 설치 확인
rpm -qa | grep openmpi

3. 환경 설정

bash

# 시스템에 설치된 OpenMPI 경로 확인
ls -l /usr/lib64/openmpi/bin/

# 모듈 파일 확인
module avail

# OpenMPI 모듈 로드
module load mpi/openmpi-x86_64

# 환경 변수 확인
which mpicc
which mpirun
mpirun --version

# 영구적으로 환경 설정 추가 (.bashrc에 추가)
cat >> ~/.bashrc << 'EOF'

# OpenMPI 설정
module load mpi/openmpi-x86_64
EOF

# 적용
source ~/.bashrc

4. Slurm과 OpenMPI 통합 설정

bash

# Slurm과 OpenMPI 통합을 위한 PMI2 지원 확인
sudo dnf install -y slurm-pmi-devel

# MPI 설정 확인 (slurm.conf 파일에 MpiDefault=pmi2 설정 확인)
grep MpiDefault /etc/slurm/slurm.conf

# MpiDefault 설정이 없다면 추가
if ! grep -q "MpiDefault" /etc/slurm/slurm.conf; then
    sudo sh -c 'echo "MpiDefault=pmi2" >> /etc/slurm/slurm.conf'
    sudo systemctl restart slurmctld
    ssh compute1 "sudo systemctl restart slurmd"
fi

5. OpenMPI 테스트 프로그램 작성

bash

# 테스트 디렉토리 생성
mkdir -p ~/mpi_test
cd ~/mpi_test

# 테스트 프로그램 작성
cat > mpi_hello.c << 'EOF'
#include <mpi.h>
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>

int main(int argc, char** argv) {
    int rank, size, len;
    char hostname[256];
    
    // MPI 초기화
    MPI_Init(&argc, &argv);
    
    // 프로세스 랭크와 전체 프로세스 수 가져오기
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    MPI_Comm_size(MPI_COMM_WORLD, &size);
    
    // 호스트명 가져오기
    gethostname(hostname, sizeof(hostname));
    
    printf("Hello from %s, rank %d out of %d processors\n", 
           hostname, rank, size);
    
    // 모든 프로세스가 출력을 완료할 시간을 줌
    MPI_Barrier(MPI_COMM_WORLD);
    
    // 랭크 0의 프로세스만 요약 정보 출력
    if (rank == 0) {
        printf("\nJob completed with %d processes\n", size);
    }
    
    MPI_Finalize();
    return 0;
}
EOF

# 컴파일
mpicc -o mpi_hello mpi_hello.c

# 컴파일 성공 확인
ls -l mpi_hello

6. 호스트 파일 설정

bash

# 호스트 파일 생성
cat > hostfile << 'EOF'
headnode slots=4
compute1 slots=4
EOF

# 실제 노드의 CPU 코어 수에 맞게 slots 값 조정
# CPU 코어 수 확인 방법
cpu_cores=$(lscpu | grep "^CPU(s):" | awk '{print $2}')
echo "Available CPU cores: $cpu_cores"

# 필요시 hostfile 수정
# vi hostfile

컴퓨트 노드 설정

SSH를 통해 컴퓨트 노드에 접속하여 아래 명령어를 실행합니다:

bash

# 로그인
ssh compute1

# 필요한 패키지 설치
sudo dnf clean all
sudo dnf makecache

# 개발 도구 설치
sudo dnf groupinstall "Development Tools" -y

# OpenMPI 및 관련 패키지 설치
sudo dnf install -y \
  environment-modules \
  gcc-gfortran \
  openmpi openmpi-devel \
  hwloc hwloc-devel \
  numactl numactl-devel

# 설치 확인
rpm -qa | grep openmpi

# 환경 설정
cat >> ~/.bashrc << 'EOF'

# OpenMPI 설정
module load mpi/openmpi-x86_64
EOF

source ~/.bashrc

# 정상 로드 확인
which mpirun

# 마스터 노드로 돌아가기
exit

테스트 단계 (마스터 노드에서)
1. 기본 MPI 실행 테스트

bash

cd ~/mpi_test

# 로컬 실행 테스트
mpirun -np 2 ./mpi_hello

# 성공 시 다음 단계로 진행

2. 클러스터 실행 테스트 (직접 mpirun 사용)

bash

# 호스트 파일을 사용한 실행
mpirun -np 4 --hostfile hostfile ./mpi_hello

# 노드 지정 실행
mpirun -np 4 -H headnode:2,compute1:2 ./mpi_hello

3. Slurm을 통한 실행 테스트

bash

# srun을 통한 실행
srun -N2 -n4 ./mpi_hello

# 또는
srun --nodes=2 --ntasks=4 ./mpi_hello

4. 배치 작업 테스트

bash

# 배치 작업 스크립트 생성
cat > job.slurm << 'EOF'
#!/bin/bash
#SBATCH --job-name=mpi_test
#SBATCH --nodes=2
#SBATCH --ntasks=4
#SBATCH --ntasks-per-node=2
#SBATCH --output=mpi_job_%j.out

# 모듈 로드
module load mpi/openmpi-x86_64

# 작업 시작 정보 출력
echo "Job started at $(date)"
echo "Running on $(hostname)"

# MPI 프로그램 실행
srun ./mpi_hello

# 작업 종료 정보 출력
echo "Job completed at $(date)"
EOF

# 작업 제출
sbatch job.slurm

# 작업 상태 확인
squeue

# 작업 결과 확인 (작업 완료 후)
cat mpi_job_*.out

문제 해결 및 최적화
1. 방화벽 설정 확인

bash

# 방화벽 상태 확인
sudo firewall-cmd --state

# 필요한 포트 개방 확인 및 추가
sudo firewall-cmd --list-all

# 필요시 포트 추가 (두 노드 모두에서 실행)
sudo firewall-cmd --permanent --add-port=1024-65535/tcp
sudo firewall-cmd --reload

2. SSH 설정 최적화

bash

# ~/.ssh/config 파일 설정 (필요시)
cat > ~/.ssh/config << 'EOF'
Host compute1
    StrictHostKeyChecking no
    UserKnownHostsFile /dev/null
    LogLevel ERROR
EOF

chmod 600 ~/.ssh/config

3. OpenMPI 고급 설정

bash

# OpenMPI 고급 설정 (TCP 기반)
cat > ~/.openmpi.conf << 'EOF'
# OpenMPI 설정
# TCP 인터페이스 설정
btl_tcp_if_include=eth0
EOF

# 실행 시 적용
mpirun --mca btl_tcp_if_include eth0 -np 4 --hostfile hostfile ./mpi_hello

4. Slurm과 OpenMPI 통합 확인 스크립트

bash

cat > ~/check_integration.sh << 'EOF'
#!/bin/bash

echo "Checking Slurm and OpenMPI integration..."

# Slurm 상태 확인
echo "Slurm status:"
sinfo

# OpenMPI 버전 확인
echo -e "\nOpenMPI version:"
mpirun --version

# Slurm과 OpenMPI 통합 테스트
echo -e "\nTesting Slurm with OpenMPI:"
srun -N1 -n1 mpirun --version

echo -e "\nTest complete."
EOF

chmod +x ~/check_integration.sh
~/check_integration.sh

최종 검증 및 성능 테스트

bash

# 간단한 성능 테스트
cat > ~/mpi_ring.c << 'EOF'
#include <mpi.h>
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char** argv) {
    int rank, size, token;
    MPI_Status status;
    double start_time, end_time;
    
    MPI_Init(&argc, &argv);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    MPI_Comm_size(MPI_COMM_WORLD, &size);
    
    // 시작 시간 기록
    start_time = MPI_Wtime();
    
    // 링 패턴으로 토큰 전송
    token = 0;
    if (rank == 0) {
        token = 42;
        printf("Process 0 sending token %d to process 1\n", token);
        MPI_Send(&token, 1, MPI_INT, 1, 0, MPI_COMM_WORLD);
        MPI_Recv(&token, 1, MPI_INT, size-1, 0, MPI_COMM_WORLD, &status);
        printf("Process 0 received token %d from process %d\n", token, size-1);
    } else {
        MPI_Recv(&token, 1, MPI_INT, rank-1, 0, MPI_COMM_WORLD, &status);
        printf("Process %d received token %d from process %d\n", 
               rank, token, rank-1);
        
        // 토큰 증가
        token++;
        
        // 다음 프로세스로 전송 (마지막은 0으로)
        int next = (rank + 1) % size;
        printf("Process %d sending token %d to process %d\n", 
               rank, token, next);
        MPI_Send(&token, 1, MPI_INT, next, 0, MPI_COMM_WORLD);
    }
    
    // 끝 시간 기록
    end_time = MPI_Wtime();
    
    // 시간 출력
    if (rank == 0) {
        printf("Total time: %f seconds\n", end_time - start_time);
    }
    
    MPI_Finalize();
    return 0;
}
EOF

# 컴파일
mpicc -o mpi_ring mpi_ring.c

# 성능 테스트 실행
srun -N2 -n4 ./mpi_ring

이 가이드를 따라하시면 OpenMPI가 성공적으로 구축되고 Slurm과 통합되어 작동할 것입니다. 각 단계에서 에러가 발생할 경우 바로 해결할 수 있도록 확인 단계를 포함했습니다. 실제 환경에 따라 일부 경로나 설정을 조정해야 할 수 있습니다.

4번   MpiDefault=pmi2 라고 나오는데 
if ~ 설정 추가해야합니까?
편집

MpiDefault=pmi2가 이미 slurm.conf 파일에 설정되어 있다면, 추가 설정이 필요하지 않습니다. 이미 Slurm과 OpenMPI의 통합을 위한 필요한 설정이 되어 있는 상태입니다.

해당 코드 블록은 조건부 설정으로, 만약 MpiDefault=pmi2 설정이 없을 경우에만 추가하라는 의미입니다:

bash

# MPI 설정 확인 (slurm.conf 파일에 MpiDefault=pmi2 설정 확인)
grep MpiDefault /etc/slurm/slurm.conf

# MpiDefault 설정이 없다면 추가
if ! grep -q "MpiDefault" /etc/slurm/slurm.conf; then
    sudo sh -c 'echo "MpiDefault=pmi2" >> /etc/slurm/slurm.conf'
    sudo systemctl restart slurmctld
    ssh compute1 "sudo systemctl restart slurmd"
fi

이 명령을 실행했을 때 MpiDefault=pmi2라고 출력된다면, 이미 설정이 되어 있는 것이므로 if 조건문 안의 내용은 실행되지 않습니다. 따라서 추가 설정을 하지 않아도 됩니다.

이미 설정되어 있으니 해당 부분은 건너뛰고 다음 단계로 진행하시면 됩니다.

    [root@headnode mpi_test]# mpirun -np 2 ./mpi_hello -------------------------------------------------------------------------- mpirun has detected an attempt to run as root. Running as root is strongly discouraged as any mistake (e.g., in defining TMPDIR) or bug can result in catastrophic damage to the OS file system, leaving your system in an unusable state. We strongly suggest that you run mpirun as a non-root user. You can override this protection by adding the --allow-run-as-root option to the cmd line or by setting two environment variables in the following way: the variable OMPI_ALLOW_RUN_AS_ROOT=1 to indicate the desire to override this protection, and OMPI_ALLOW_RUN_AS_ROOT_CONFIRM=1 to confirm the choice and add one more layer of certainty that you want to do so. We reiterate our advice against doing so - please proceed at your own risk. --------------------------------------------------------------------------

편집

이 메시지는 root 사용자로 mpirun을 실행하는 것이 권장되지 않는다는 경고입니다. OpenMPI에서는 보안 및 시스템 안전성을 위해 일반 사용자로 실행하는 것을 권장합니다.

해결 방법은 다음과 같습니다:
방법 1: 일반 사용자로 전환하여 실행 (권장)

일반 사용자 계정을 만들고 해당 계정으로 전환하여 MPI 작업을 실행하는 것이 가장 안전한 방법입니다:

bash

# mpiuser 사용자 생성
useradd -m mpiuser
passwd mpiuser  # 비밀번호 설정

# 필요한 디렉토리와 파일 권한 설정
cp mpi_hello /home/mpiuser/
chown mpiuser:mpiuser /home/mpiuser/mpi_hello

# mpiuser로 전환
su - mpiuser

# MPI 환경 설정
module load mpi/openmpi-x86_64

# 실행
mpirun -np 2 ./mpi_hello

방법 2: 환경 변수 설정 (임시 해결책)

일시적으로 root로 실행해야 하는 경우에는 다음 환경 변수를 설정하여 경고를 우회할 수 있습니다:

bash

# 환경 변수 설정
export OMPI_ALLOW_RUN_AS_ROOT=1
export OMPI_ALLOW_RUN_AS_ROOT_CONFIRM=1

# 실행
mpirun -np 2 ./mpi_hello

방법 3: 명령행 옵션 사용 (임시 해결책)

명령행에 --allow-run-as-root 옵션을 추가하여 실행:

bash

mpirun --allow-run-as-root -np 2 ./mpi_hello

주의사항

방법 2와 방법 3은 임시 해결책으로만 사용하세요. 프로덕션 환경에서는 방법 1과 같이 일반 사용자로 MPI 작업을 실행하는 것이 안전합니다. root로 실행하면 실수로 인해 시스템에 심각한 손상을 줄 수 있습니다.

또한, Slurm을 통해 작업을 제출할 때에도 일반 사용자 계정으로 제출하는 것이 좋습니다:

bash

# 일반 사용자로 Slurm 작업 제출
su - mpiuser
srun -N2 -n4 ./mpi_hello

HPC 환경에서는 보통 관리자가 시스템을 설정하고, 사용자들은 일반 계정으로 작업을 제출하는 방식으로 운영합니다.

[mpiuser@headnode ~]$ mpirun -np 4 --hostfile hostfile ./mpi_hello
Ignoring PCI device with non-16bit domain.
Pass --enable-32bits-pci-domain to configure to support such devices
(warning: it would break the library ABI, don't enable unless really needed).
^C[mpiuser@headnode ~]$ 
[mpiuser@headnode ~]$ mpirun -np 4 -H headnode:2,computenode:2 ./mpi_hello
Ignoring PCI device with non-16bit domain.
Pass --enable-32bits-pci-domain to configure to support such devices
(warning: it would break the library ABI, don't enable unless really needed).
편집

이 문제는 PCI 장치 도메인 관련 경고 메시지이며, 실행이 진행되지 않고 멈춘 것으로 보입니다. 이 경고 자체는 일반적으로 무시해도 되는 메시지이지만, 프로그램이 실행되지 않는 것은 다른 문제가 있을 수 있습니다.

다음 단계들을 통해 문제를 해결해 보겠습니다:
1. 기본 연결 테스트

먼저 호스트 간 SSH 연결이 비밀번호 없이 가능한지 확인해 보세요:

bash

# SSH 키가 설정되어 있는지 확인
ssh computenode hostname

# 연결이 되지 않는다면 SSH 키를 설정
ssh-keygen -t rsa -b 4096 -N ""
ssh-copy-id computenode

2. hostfile 내용 확인

현재 hostfile의 내용을 확인하고 필요하면 수정합니다:

bash

# hostfile 내용 확인
cat hostfile

# 필요시 다음과 같이 수정
echo "headnode slots=2" > hostfile
echo "computenode slots=2" >> hostfile

3. 단순화된 테스트

문제를 격리하기 위해 로컬에서만 실행해 봅니다:

bash

# 로컬에서만 실행
mpirun -np 2 ./mpi_hello

4. 디버깅 정보 확인

OpenMPI에 디버깅 정보를 출력하도록 설정:

bash

# 상세 디버깅 정보로 실행
mpirun --verbose -np 4 -H headnode:2,computenode:2 ./mpi_hello

5. SSH 디버깅 정보 확인

SSH 연결 문제를 확인하기 위해:

bash

# SSH 디버깅 모드로 연결 테스트
ssh -v computenode hostname

6. OpenMPI 설정 변경

OpenMPI가 사용하는 통신 방식을 변경해 봅니다:

bash

# TCP만 사용하도록 설정
mpirun --mca btl tcp,self -np 4 -H headnode:2,computenode:2 ./mpi_hello

# 또는 SSH 모듈 사용 (원격 시작 방식 변경)
mpirun --mca plm_rsh_agent ssh -np 4 -H headnode:2,computenode:2 ./mpi_hello

7. 호스트 이름 확인

hostfile과 명령어에서 사용한 computenode 이름이 실제 호스트 이름과 일치하는지 확인:

bash

# 호스트 이름 확인
hostname
ssh computenode hostname

# /etc/hosts 파일 확인
cat /etc/hosts

8. Slurm을 통한 실행

Slurm을 사용하여 실행해보는 것도 좋은 방법입니다:

bash

# Slurm을 통한 실행
srun -N2 -n4 ./mpi_hello

9. 타임아웃 설정

연결 타임아웃을 늘려봅니다:

bash

mpirun --mca plm_rsh_no_tree_spawn 1 --mca orte_startup_timeout 100 -np 4 -H headnode:2,computenode:2 ./mpi_hello

10. 방화벽 확인

방화벽이 MPI 통신을 차단하고 있는지 확인:

bash

# 방화벽 상태 확인
sudo systemctl status firewalld

# 필요시 방화벽 비활성화하여 테스트
sudo systemctl stop firewalld

11. OpenMPI의 버전 확인 및 일관성 검사

두 노드의 OpenMPI 버전이 동일한지 확인:

bash

# 로컬 버전 확인
mpirun --version

# 원격 버전 확인
ssh computenode "mpirun --version"

12. 사용자 권한 확인

computenode에서도 mpiuser 사용자가 제대로 설정되었는지 확인:

bash

# 원격 노드에서 사용자 확인
ssh computenode "id mpiuser"

# 필요시 원격 노드에 사용자 생성
ssh root@computenode "useradd -m mpiuser && passwd mpiuser"

이 단계들을 차례로 진행해보시고, 어떤 단계에서 문제가 해결되는지 확인해 보세요. PCI 장치 경고는 특별한 문제가 아니라 하드웨어 관련 정보 메시지이므로, 실행되지 않는 주요 원인은 네트워크 연결, SSH 설정, 권한 문제 중 하나일 가능성이 높습니다.

#include

Our most intelligent model yet 자세히 알아보기
콘텐츠
아직 추가된 콘텐츠가 없습니다.
Claude와 함께 이미지, PDF, 문서, 스프레드시트 등을 추가하여 콘텐츠를 요약하고 분석하며 검색할 수 있습니다.
