# ✅ Red Hat HPC Head Node 구축 완료 내역 (RHEL 9.5 기준)

## 📦 시스템 설치 및 초기 설정

- 설치 버전: **Red Hat Enterprise Linux 9.5 (x86_64)**
- 부팅 설정: **UEFI 모드**, **GPT 파티션**
- 설치 ISO: `rhel-9.5-x86_64-dvd.iso`
- 설치 환경: **Workstation**
- 네트워크 연결: ✅ 성공

---

## 🔧 시스템 업데이트 및 개발 도구 설치

```bash
sudo dnf update -y
sudo dnf groupinstall "Development Tools" -y


sudo dnf install -y munge munge-libs
sudo /usr/sbin/create-munge-key
sudo systemctl enable munge --now
```

<!DOCTYPE html>
<html lang="en">
<body>
  <h1>HPC 환경 설정 - Head Node & Compute Node</h1>

  <h2>1. Head Node 설정</h2>

  <h3>1.1. 시스템 업데이트</h3>
  <div class="code-block">
    <button class="copy-btn" onclick="copyCode(this)">복사</button>
    <pre id="code1"><code>sudo dnf update -y</code></pre>
  </div>

  <h3>1.2. 필수 패키지 설치</h3>
  <div class="code-block">
    <button class="copy-btn" onclick="copyCode(this)">복사</button>
    <pre id="code2"><code>sudo dnf install -y openmpi openmpi-devel lmod environment-modules munge munge-libs munge-devel</code></pre>
  </div>

  <h3>1.3. Munge 키 생성</h3>
  <div class="code-block">
    <button class="copy-btn" onclick="copyCode(this)">복사</button>
    <pre id="code3"><code>sudo /usr/sbin/create-munge-key</code></pre>
  </div>

  <h3>1.4. Munge 서비스 시작</h3>
  <div class="code-block">
    <button class="copy-btn" onclick="copyCode(this)">복사</button>
    <pre id="code4"><code>sudo systemctl start munge
sudo systemctl enable munge</code></pre>
  </div>

  <h3>1.5. Slurm 컨트롤러 설정</h3>
  <div class="code-block">
    <button class="copy-btn" onclick="copyCode(this)">복사</button>
    <pre id="code5"><code>sudo cp /etc/slurm/slurm.conf.example /etc/slurm/slurm.conf</code></pre>
  </div>

  <h3>1.6. Slurm 컨트롤러 서비스 시작</h3>
  <div class="code-block">
    <button class="copy-btn" onclick="copyCode(this)">복사</button>
    <pre id="code6"><code>sudo systemctl start slurmctld
sudo systemctl enable slurmctld</code></pre>
  </div>

  <h2>2. Compute Node 설정</h2>

  <h3>2.1. Munge 키 복사</h3>
  <div class="code-block">
    <button class="copy-btn" onclick="copyCode(this)">복사</button>
    <pre id="code7"><code>sudo scp /etc/munge/munge.key root@<compute-node-ip>:/etc/munge/</code></pre>
  </div>

  <h3>2.2. Slurm Daemon 설치</h3>
  <div class="code-block">
    <button class="copy-btn" onclick="copyCode(this)">복사</button>
    <pre id="code8"><code>sudo dnf install -y slurm slurm-devel slurm-munge slurm-plugins</code></pre>
  </div>

  <h3>2.3. Slurm 서비스 시작</h3>
  <div class="code-block">
    <button class="copy-btn" onclick="copyCode(this)">복사</button>
    <pre id="code9"><code>sudo systemctl start slurmd
sudo systemctl enable slurmd</code></pre>
  </div>

  <h2>3. 작업 제출</h2>

  <h3>3.1. 간단한 작업 제출</h3>
  <div class="code-block">
    <button class="copy-btn" onclick="copyCode(this)">복사</button>
    <pre id="code10"><code>sbatch test_job.sh</code></pre>
  </div>

  <h3>3.2. 배치 작업 스크립트 (test_job.sh)</h3>
  <div class="code-block">
    <button class="copy-btn" onclick="copyCode(this)">복사</button>
    <pre id="code11"><code>#!/bin/bash
#SBATCH --job-name=test
#SBATCH --output=output.txt
#SBATCH --time=00:01:00

echo "Hello from compute node"
hostname</code></pre>
  </div>

  <h2>4. 시스템 상태 확인</h2>

  <h3>4.1. 노드 상태 확인</h3>
  <div class="code-block">
    <button class="copy-btn" onclick="copyCode(this)">복사</button>
    <pre id="code12"><code>sinfo</code></pre>
  </div>

  <h3>4.2. 작업 큐 상태 확인</h3>
  <div class="code-block">
    <button class="copy-btn" onclick="copyCode(this)">복사</button>
    <pre id="code13"><code>squeue</code></pre>
  </div>

  <h2>5. 정상 동작 확인</h2>

  <h3>5.1. 결과 파일 확인</h3>
  <p>배치 작업이 정상적으로 완료되었을 경우, 지정된 출력 파일(`output.txt`)에 결과가 기록됩니다.</p>

  <h2>6. 결론</h2>
  <p>Head Node와 Compute Node가 성공적으로 연결되어 Slurm 클러스터가 정상적으로 동작하고 있으며, 분산 컴퓨팅 환경에서 배치 작업을 실행할 수 있는 상태입니다.</p>

  <script>
    function copyCode(button) {
      const codeBlock = button.nextElementSibling.querySelector('code');
      const range = document.createRange();
      range.selectNode(codeBlock);
      window.getSelection().removeAllRanges();
      window.getSelection().addRange(range);
      document.execCommand('copy');
    }
  </script>
</body>
</html>
