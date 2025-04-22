# HPC 환경 설정 - Head Node & Compute Node

## 1. Head Node 설정
### 1.1. 시스템 업데이트
Head Node에서 시스템을 최신 상태로 업데이트합니다.
```bash
sudo dnf update -y
1.2. 필수 패키지 설치
Slurm 및 Munge 관련 패키지들을 설치합니다.

bash
복사
편집
sudo dnf install -y openmpi openmpi-devel lmod environment-modules munge munge-libs munge-devel
1.3. Munge 키 생성
Munge 인증 시스템을 설정하여 Munge 키를 생성합니다.

bash
복사
편집
sudo /usr/sbin/create-munge-key
1.4. Munge 서비스 시작
Munge 인증 서비스를 시작하고 활성화합니다.

bash
복사
편집
sudo systemctl start munge
sudo systemctl enable munge
1.5. Slurm 컨트롤러 설정
Slurm 컨트롤러 설정 파일(slurm.conf)을 /etc/slurm/slurm.conf.example에서 복사하여 설정합니다.

bash
복사
편집
sudo cp /etc/slurm/slurm.conf.example /etc/slurm/slurm.conf
1.6. Slurm 컨트롤러 서비스 시작
Slurm 컨트롤러 서비스를 시작하고 활성화합니다.

bash
복사
편집
sudo systemctl start slurmctld
sudo systemctl enable slurmctld
2. Compute Node 설정
2.1. Munge 키 복사
Munge 키를 Compute Node로 복사하여 인증 시스템을 동일하게 설정합니다.

bash
복사
편집
sudo scp /etc/munge/munge.key root@<compute-node-ip>:/etc/munge/
2.2. Slurm Daemon 설치
Slurm 관련 서비스를 설치하고 설정을 완료합니다.

bash
복사
편집
sudo dnf install -y slurm slurm-devel slurm-munge slurm-plugins
2.3. Slurm 서비스 시작
Slurm 서비스를 시작하고 활성화합니다.

bash
복사
편집
sudo systemctl start slurmd
sudo systemctl enable slurmd
3. 작업 제출
3.1. 간단한 작업 제출
sbatch를 사용하여 배치 작업을 제출하고, squeue로 작업 상태를 확인합니다.

bash
복사
편집
sbatch test_job.sh
3.2. 배치 작업 스크립트 (test_job.sh)
bash
복사
편집
#!/bin/bash
#SBATCH --job-name=test
#SBATCH --output=output.txt
#SBATCH --time=00:01:00

echo "Hello from compute node"
hostname
4. 시스템 상태 확인
4.1. 노드 상태 확인
Slurm 시스템에서 현재 상태를 확인할 수 있는 명령어입니다.

bash
복사
편집
sinfo
4.2. 작업 큐 상태 확인
현재 실행 중인 작업들을 확인할 수 있습니다.

bash
복사
편집
squeue
5. 정상 동작 확인
5.1. 결과 파일 확인
배치 작업이 정상적으로 완료되었을 경우, 지정된 출력 파일(output.txt)에 결과가 기록됩니다.

6. 결론
Head Node와 Compute Node가 성공적으로 연결되어 Slurm 클러스터가 정상적으로 동작하고 있으며, 분산 컴퓨팅 환경에서 배치 작업을 실행할 수 있는 상태입니다.
