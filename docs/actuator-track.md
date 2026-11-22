# 구동 · 드라이버 갈래 — 모터 쪽 통신이 끊기면 무엇을 하나

**한 줄 질문**: 로봇 컴퓨터와 모터 사이(USB · DYNAMIXEL 버스)가 끊기면 드라이버와 모터는 무엇을 하나, 그리고 그것을 실물 없이도 시험할 수 있나.

## 이야기 순서

1. **고장 ②: 드라이버가 낡은 값을 낸다**: TurtleBot3 드라이버가 OpenCR 읽기에 실패하면 마지막 값을 지금 시각으로 계속 낸다(소스 기준) → 받침대에서 확인 → `read_timeout` 패치(0 = 끔) → 전 · 후.
2. **알리기**: 고장 상태를 `/diagnostics`로 낸다. 상태 판단은 ROS와 나눈 C++ 상태 전이 함수(`fault_state`)가 한다.
3. **ros2_control로 돌릴 때**: `usb_to_dxl` + `dynamixel_hardware_interface` + `diff_drive_controller`에서 명령 끊김 · USB 끊김을 넣고, 모터의 Bus Watchdog을 켤 때 · 끌 때를 견준다. 위 드라이버 패치와 한 표로 둔다.
4. **가짜 장치**: 가상 직렬 포트에서 Protocol 2.0 장치의 응답을 흉내 내어, 실물 없이 같은 고장을 CI에서 다시 낸다.

## 관련 문서

- 고장 표: [fault-table.md](fault-table.md)의 F5 · F5-rc
- 요구사항: [requirements.md](requirements.md)의 REQ-STALE-1 · REQ-DIAG-1 · REQ-OFF-1
- 시험 절차: [test-procedure.md](test-procedure.md)
