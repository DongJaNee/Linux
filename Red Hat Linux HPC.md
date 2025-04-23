<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Slurm HPC 환경 설정 가이드</title>
</head>
<body>
    <h1>Slurm HPC 환경 설정 가이드</h1>
    <p>이 가이드는 Head Node와 Compute Node를 설정하여 Slurm 클러스터를 구축하는 방법을 다룹니다. 각 단계별로 발생할 수 있는 오류를 피하기 위한 조언도 포함되어 있습니다.</p>

    <h2>1. Head Node 설정</h2>

    <h3>1.1. 시스템 업데이트</h3>
    <pre><code>sudo dnf update -y</code></pre>

    <h3>1.2. 필수 패키지 설치</h3>
    <pre><code>sudo dnf install -y openmpi openmpi-devel lmod environment-modules munge munge-libs munge-devel slurm slurm-devel slurm-munge</code></pre>

    <h3>1.3. Munge 키 생성</h3>
    <pre><code>sudo /usr/sbin/create-munge-key</code></pre>

    <h3>1.4. Munge 서비스 시작</h3>
    <pre><code>sudo systemctl start munge
sudo systemctl enable munge</code></pre>

    <h3>1.5. Slurm 컨트롤러 설정</h3>
    <pre><code>sudo cp /etc/slurm/slurm.conf.example /etc/slurm/slurm.conf</code></pre>

    <h3>1.6. Slurm 컨트롤러 서비스 시작</h3>
    <pre><code>sudo systemctl start slurmctld
sudo systemctl enable slurmctld</code></pre>

    <h3>1.7. Slurm 상태 확인</h3>
    <pre><code>sudo systemctl status slurmctld</code></pre>

    <hr>

    <h2>2. Compute Node 설정</h2>

    <h3>2.1. Munge 키 복사</h3>
    <pre><code>sudo scp /etc/munge/munge.key root@&lt;compute-node-ip&gt;:/etc/munge/</code></pre>

    <h3>2.2. Slurm Daemon 설치</h3>
    <pre><code>sudo dnf install -y slurm slurm-devel slurm-munge slurm-plugins</code></pre>

    <h3>2.3. Slurm 서비스 시작</h3>
    <pre><code>sudo systemctl start slurmd
sudo systemctl enable slurmd</code></pre>

    <h3>2.4. Compute Node 상태 확인</h3>
    <pre><code>sudo systemctl status slurmd</code></pre>

    <hr>

    <h2>3. Head Node와 Compute Node 연결</h2>

    <h3>3.1. Slurm 노드 설정</h3>
    <p>slurm.conf에서 노드 정보를 설정합니다. Head Node와 Compute Node에서 동일한 노드 이름을 사용해야 합니다. 예시:</p>
    <pre><code>NodeName=localhost CPUs=4 RealMemory=8000 Sockets=1 CoresPerSocket=2 ThreadsPerCore=1 State=UNKNOWN
PartitionName=debug Nodes=localhost Default=YES MaxTime=INFINITE State=UP</code></pre>

    <h3>3.2. Slurm 상태 확인</h3>
    <pre><code>sinfo</code></pre>
    <p>Compute Node가 <code>UP</code> 상태여야 합니다.</p>

    <hr>

    <h2>4. 작업 제출 및 상태 확인</h2>

    <h3>4.1. 테스트 작업 제출</h3>
    <pre><code>sbatch test_job.sh</code></pre>

    <h3>4.2. 배치 작업 스크립트 (test_job.sh)</h3>
    <pre><code>#!/bin/bash
#SBATCH --job-name=test
#SBATCH --output=output.txt
#SBATCH --time=00:10:00

echo "Hello from compute node"
hostname</code></pre>

    <h3>4.3. 작업 상태 확인</h3>
    <pre><code>squeue</code></pre>

    <h3>4.4. 결과 확인</h3>
    <pre><code>cat output.txt</code></pre>

    <hr>

    <h2>5. 문제 해결</h2>

    <h3>5.1. Munge 서비스 오류</h3>
    <p><code>Munge</code> 서비스가 제대로 실행되지 않으면 다음 명령어로 확인하고 재시작할 수 있습니다.</p>
    <pre><code>sudo systemctl status munge
sudo systemctl restart munge</code></pre>

    <h3>5.2. Slurmctld 서비스 오류</h3>
    <p>Slurm 컨트롤러가 실행되지 않으면 로그를 확인합니다.</p>
    <pre><code>sudo journalctl -u slurmctld</code></pre>
    <p>로그에서 <code>error</code> 메시지를 확인하고 <code>slurm.conf</code> 파일을 다시 점검하여 수정합니다.</p>

    <h3>5.3. Slurm 상태 오류</h3>
    <p><code>slurmd</code> 서비스가 실행되지 않거나 <code>scontrol</code>에서 오류가 발생하면, <code>slurmd</code> 서비스 로그를 확인합니다.</p>
    <pre><code>sudo journalctl -u slurmd</code></pre>

    <h3>5.4. SSH 연결 오류</h3>
    <p><code>ssh</code> 연결이 안될 경우, <code>sshd_config</code>에서 <code>PermitRootLogin yes</code>와 <code>PasswordAuthentication yes</code>가 설정되어 있는지 확인합니다. 이후 SSH 서비스를 재시작합니다.</p>
    <pre><code>sudo systemctl restart sshd</code></pre>

    <hr>

    <h2>6. 결론</h2>
    <p>이 가이드를 따라 설정한 후, Head Node와 Compute Node가 성공적으로 연결되고, Slurm을 통해 작업을 스케줄링하여 실행할 수 있습니다. 모든 오류는 <code>journalctl</code> 명령어로 로그를 확인하고 <code>slurm.conf</code> 파일을 점검하는 방식으로 해결할 수 있습니다.</p>
</body>
</html>
