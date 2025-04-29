# 컴퓨트 노드 구성 오류 요약

## 1. Slurm 데몬 시작 실패

### 오류:


### 상세 내용:
- Slurm 데몬(`slurmd`) 서비스가 여러 번 시작되지 않았습니다. 주된 원인은 잘못된 설정이었습니다.
- `slurm.conf` 파일에서 호스트 이름 불일치와 설정 오류가 주요 문제였습니다.

### 해결 방법:
- 호스트 이름이 올바르게 설정되었는지 확인하고, `slurm.conf` 파일에 정의된 `NodeName`과 일치하는지 검토합니다.
- `/etc/hosts` 파일을 수정하여 올바른 호스트 이름 매핑을 추가합니다.

## 2. 중복된 구성 항목

### 오류:

### 상세 내용:
- `slurm.conf` 파일에 여러 설정 항목이 중복되어 정의되어, Slurm은 가장 최신 값만 사용하게 됩니다.

### 해결 방법:
- `slurm.conf` 파일을 검토하여 각 항목이 한 번만 정의되도록 정리합니다.  
  예시:
  - `MpiDefault`, `ProctrackType` 등의 항목이 중복되지 않도록 제거합니다.

## 3. SlurmctldHost 설정 충돌

### 오류:

### 상세 내용:
- `slurm.conf` 파일에서 `ControlMachine`과 `SlurmctldHost`가 모두 설정되어 있어, Slurm은 `ControlMachine`을 무시합니다.

### 해결 방법:
- `slurm.conf` 파일에서 `SlurmctldHost`만 사용하여 Slurm 컨트롤러 노드를 지정합니다.

## 4. 노드 이름을 확인할 수 없음

### 오류:

### 상세 내용:
- `slurm.conf` 파일에 컴퓨트 노드의 `NodeName`이 올바르게 지정되지 않았습니다.

### 해결 방법:
- 컴퓨트 노드의 호스트 이름에 맞는 정확한 `NodeName`을 `slurm.conf` 파일에 설정합니다. (예: `NodeName=computenode`).
