# Auto Scaling

애플리케이션의 수요에 따라 EC2 인스턴스를 자동으로 확장하고 축소하는 기능

사용자가 정의한 조정 정책에따라 인스턴스 수가 증가되거나 축소됨

- 서버의 로드가 증가하면 EC2 인스턴스 개수 추가(Scale out)
- 서버의 로드가 감소하면 EC2 인스턴스 개수 감소 (Scale in)

애플리케이션의 수요에 EC2를 자동으로 확장 및 축소하므로 비용 절감 및 수동으로 EC2 용량을 프로비저닝해야 할 필요가 없음

손상된 Amazon EC2 인스턴스를 탐지하고 Auto Scaling 이 자동으로 교체

여러 가용 영역을 사용하도록 EC2 Auto Scaling을 구성 하여 하나의 가용 영역이 사용불가 상태가 되면 다른 가용영역에서 새 인스턴스를 시작

**조정 정책**

항상 현재 인스턴스 수준 유지 관리

- 지정된 수의 실행 인스턴스를 항상 유지하도록 Auto Scaling 그룹을 구성
- 인스턴스가 비정상 상태임을 확인하면 해당 인스턴스를 종료한 다음 새 인스턴스를 시작

**수동 조정**

- 최대, 최소 또는 원하는 용량의 변경 사항만 지정하는 경우 사용

일정을 기반으로 저정 (Scheduled Scaling)

- 확장 작업이 시간 및 날짜 함수에 따라 자동으로 수행됨

온디맨드 기반 조정 (Dynamic Scaling)

- 수요 변화에 맞춰 Auto Scaling 그룹의 크기를 동적으로 조정

예측 조정 사용 (Predictive Scaling)

- 머신러닝을 사용하여 CloudWatch의 기록 데이터를 기반으로 용량 필요량을 예측

**동적 조정 (Dynamic Scaling)**

대상 추적 조정(Target Tracking Scaling)

- 지정한 지표 유형의 대상값을 기준으로 Auto Scaling 그룹을 조정 하는 방식

단계 조정(Step Scaling)

- CloudWatch alarm의 지표를 기반으로 Auto Scaling 그룹을 확장 하는 방식

단순 조정(Simple Scaling)

- CloudWatch alarm의 지표를 기반으로 Auto Scaling 그룹을 확장 하는 방식

Amazon SQS 기반 크기 조정

- Amazon SQS 대기열의 시스템 로드 변경에 따라 Auto Scaling 그룹을 조정

**조정 휴지(Scaling Cooldowns) , 인스턴스 워밍업 (Instance Warmup)**

---

- 불필요한 EC2 인스턴스가 생성되거나 종료되는 것을 방지 하는 기능
- EC2가 안정적인 서비스 상태가 될 때까지 스케일링을 하지 않도록 차단하는 역할을 함 • EC2인스턴스가처음시작된다음안정적인서비스상태가될때까지시간이소요됨

조정 휴지(Scaling Cooldowns)

- 단순조정(Simple Scaling) 정책에만 적용
- EC2가 증가 또는 감소하는 활동이 발생하면 조정 휴지 기간(Cooldown Period)을 가짐 ✓ 조정 휴지 기간 동안 Auto Scaling Group은 EC2를 종료하거나 시작하지 않음
- 디폴트 조정 휴지 기간은 300초

인스턴스 워밍업(Instance Warmup)

- 단계조정(Step Scaling), 대상추적조정(Target Tracking Scaling) 정책에만 적용
- 인스턴스가 처음시작 되면 워밍업(Warmup) 시간을 가짐
- 워밍업 시간이 만료될 때까지 인스턴스는 오토 스케일링의 집계된 EC2 인스턴스 지표에 포함되지 않음 ✓ 인스턴스 워밍업은 기본적으로 활성화 되어 있지 않음
