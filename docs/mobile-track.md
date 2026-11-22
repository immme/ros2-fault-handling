# 이동 · 통신 갈래 — 명령과 Wi-Fi가 끊기면 무엇을 하나

**한 줄 질문**: 노트북(Nav2 · teleop)에서 로봇으로 가는 명령이 끊기거나 늦어지면 로봇은 무엇을 하나, 그리고 몇 초 · 몇 cm 안에 서게 하려면 값을 어떻게 골라야 하나.

## 이야기 순서

1. **고장 ①: 명령이 끊겨도 계속 간다**: 이미 있는 방어 장치(Nav2 · 드라이버 · 펌웨어)가 어디까지 막는지 시뮬과 실물로 확인 → 받침대에서 재현([turtlebot3 #1154](https://github.com/ROBOTIS-GIT/turtlebot3/issues/1154)에 먼저 보고된 문제) → `cmd_vel_timeout` 패치(0 = 끔) → 전 · 후.
2. **값 고르기**: 보통 · 몰려 오는 손실 · 2초 끊김 · Wi-Fi 혼잡 · 로봇 CPU 부하에서 명령 간격 분포를 재고, 시간 초과 후보마다 정지 시간 · 잘못 멈춘 횟수를 견준다.
3. **실물에 맞춘 시뮬**: 시험 구역을 잰 Gazebo world, 실물에서 잰 가감속 · 지연으로 맞춘 바퀴 모델, 맞추기 전 · 후 차이 표. 맞춘 시뮬로 시간 초과 · 최고 속도를 고르고 실물로 확인한다.
4. **통신 상태**: 통신 기준선, `link_monitor`로 통신 상태를 `/diagnostics`로 알리기, Nav2로 집 안을 도는 순찰과 Wi-Fi 지도.

## 관련 문서

- 고장 표: [fault-table.md](fault-table.md)의 F2 · F3 · N2
- 요구사항: [requirements.md](requirements.md)의 REQ-STOP-1 · REQ-STOP-2 · REQ-FP-1 · REQ-SIM-1 · REQ-NET-2
- 시험 절차: [test-procedure.md](test-procedure.md)
