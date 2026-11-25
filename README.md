# 스마트 팩토리 컨베이어 라인 제어 및 모니터링 시스템 (타이어 생산 라인)

컨베이어 설비(라즈베리파이·아두이노)와 연동하여 타이어 검사 결과를 수집하고, 장비 상태 확인·모니터링·채팅/인수인계를 수행하는 스마트 팩토리 팀 프로젝트입니다.

- 기간: 2026.10.16 ~ 2026.10.28
- 인원: 5명 (팀장 1, 부팀장 1, 팀원 3) — 팀 프로젝트

## 구성

| 폴더 | 내용 | 기술 |
|---|---|---|
| `TireProductionServer/` | TCP 중앙 서버, 검사·장비 이벤트 중계 및 저장 | C#, .NET 8, SQLite |
| `WinForms/` | 장비 HMI (로그인, 검사 모니터링, 장비 제어, 채팅) | C#, WinForms, .NET 8 |
| `MFC/` | 관리자 클라이언트 (대시보드, 검사 이력, 통계, 일일 보고, 채팅·인수인계) | C++, MFC |
| `RaspberryPi/` | 카메라 서버, 라인 제어, LED·서보·부저 하드웨어 제어 | Python, Flask, OpenCV, gpiozero |

## 담당 파트 (유동호)

- **하트비트 연결 상태 확인**
  - 라즈베리파이 → 서버 하트비트 송신: `RaspberryPi/dual_camera_server.py` (`heartbeat_worker`), 테스트용 `heartbeat_test_pi.py`
  - 수신 측 5초 무수신 시 연결 끊김 판정: `WinForms/MainForm.cs` (`CheckHeartbeatTimeout`), `MFC/MainFrame.cpp` (`OnHeartbeatReceived`)
- **라즈베리파이 제어 코드 병합**: LED 제어 클래스 작성, 팀원 제어 코드 병합, 부저 동작 시 메인 루프가 멈추지 않도록 스레드 적용 (`RaspberryPi/tire_line_hardware.py`)
- **MFC 채팅 연동**: C# 서버와 연동하는 MFC 채팅 화면 (`MFC/Pages/ChatHandoverPage.cpp`)

## 실행

1. `TireProductionBase.sln`을 Visual Studio 2022로 엽니다.
2. `TireProductionServer`를 먼저 실행합니다 (기본 포트 5000).
3. `WinForms` 또는 `MFC` 클라이언트를 실행합니다. 서버 IP는 `MFC/client_config.ini`, `WinForms/client_config.json`에서 설정합니다.
4. 라즈베리파이 없이 하트비트만 테스트하려면 `python heartbeat_test_pi.py`를 실행합니다.