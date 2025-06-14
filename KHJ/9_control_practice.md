이 문서는 주어진 로봇 스펙을 바탕으로 특정 상황에서의 제어 절차 및 이동 계산 결과를 설명합니다.

---

## 1. 운반 로봇 제어 절차 (Control Procedure for Transport Robot)

**상황**: 주행 중 000으로 유지되던 IR 센서(IR Sensor) 측정값이 100으로 변경되었습니다. 이는 로봇이 경로의 왼쪽으로 이탈했음을 의미합니다. 경로(Path)는 검은색(0)이고, 흰색(1) 영역을 감지한 것입니다.

**제어 절차(Control Procedure)**:

1.  **상태 인식 (State Recognition)**:
    * 로봇은 3개의 IR 센서 어레이(Sensor Array)를 통해 5cm 앞의 경로 상태를 지속적으로 감지합니다.
    * 센서 측정값이 100으로 변경된 것은 로봇의 왼쪽 센서가 흰색 영역을 감지하고, 가운데 및 오른쪽 센서가 검은색 영역을 감지하고 있다는 것을 의미합니다.
    * 이는 로봇이 현재 경로에서 왼쪽으로 벗어나고 있음을 나타냅니다.

2.  **판단 및 목표 설정 (Decision and Goal Setting)**:
    * 로봇은 경로를 벗어났으므로, 경로 중앙으로 다시 복귀해야 합니다.
    * 이를 위해 로봇은 오른쪽으로 회전하여 원래의 검은색 경로를 다시 찾아야 합니다.

3.  **제어 명령 생성 (Control Command Generation)**:
    * 경로 중앙으로 복귀하기 위해, 로봇은 **오른쪽 바퀴(Right Wheel)의 속도를 높이거나, 왼쪽 바퀴(Left Wheel)의 속도를 줄여서 오른쪽으로 회전하는 명령을 생성합니다.**
    * 가장 간단한 제어 방법은 PID 제어(PID Control)와 같은 피드백 제어(Feedback Control)를 사용하는 것입니다. 여기서는 간단히 속도 조절을 가정합니다.
    * **가정**: 로봇은 정해진 속도(Speed)로 직진(Straight Line) 주행하고 있으며, 센서 값의 변화에 따라 즉시 속도 제어를 수행합니다.

4.  **액추에이터 제어 (Actuator Control)**:
    * 생성된 제어 명령에 따라 오른쪽 바퀴의 모터(Motor)에 더 많은 동력을 공급하거나, 왼쪽 바퀴의 모터에 동력을 덜 공급하여 오른쪽으로 회전하도록 합니다.
    * 예를 들어, 왼쪽 바퀴의 속도를 $V_L$, 오른쪽 바퀴의 속도를 $V_R$이라고 할 때, $V_L > V_R$이 되도록 제어합니다.

5.  **피드백 및 반복 (Feedback and Iteration)**:
    * 로봇은 회전하면서 IR 센서 값을 다시 확인합니다.
    * 센서 값이 000으로 돌아올 때까지 위의 절차를 반복합니다. 000이 되면 로봇은 다시 직진 주행 모드로 전환합니다.
    * 이 과정은 로봇이 경로 중앙을 지속적으로 추적할 수 있도록 해줍니다.

---

## 2. 로봇 이동 설명 및 계산 (Robot Movement Description and Calculation)

**로봇 스펙(Robot Specifications)**:
* 바퀴 지름(Wheel Diameter): $D = 10 \text{ cm}$
* 엔코더 해상도(Encoder Resolution): $E = 30 \text{ pulses/revolution}$
* 두 바퀴 사이의 거리(Distance Between Wheels): $L = 20 \text{ cm}$

**계산에 필요한 기본값(Basic Values for Calculation)**:
* 바퀴 둘레(Wheel Circumference): $C = \pi D = 10\pi \text{ cm}$
* 1 펄스당 이동 거리(Distance per Pulse): $d_p = C / E = (10\pi \text{ cm}) / 30 = \pi/3 \text{ cm/pulse}$

**주행 상황 분석(Driving Situation Analysis)**:

1.  **초기 직진 구간 (Initial Straight Driving Section)**:
    * 기간: 100 펄스(pulse)
    * 양쪽 바퀴 엔코더 측정값 동일.
    * 이동 거리: $100 \text{ pulses} \times (\pi/3 \text{ cm/pulse}) = 100\pi/3 \text{ cm} \approx 104.72 \text{ cm}$
    * 회전 각도: $0^\circ$ (직진 이동)

2.  **회전 구간 (Turning Section)**:
    * 기간: 왼쪽 바퀴(Left Wheel) 20 펄스, 오른쪽 바퀴(Right Wheel) 10 펄스
    * 왼쪽 바퀴 이동 거리($d_L$): $20 \text{ pulses} \times (\pi/3 \text{ cm/pulse}) = 20\pi/3 \text{ cm} \approx 20.94 \text{ cm}$
    * 오른쪽 바퀴 이동 거리($d_R$): $10 \text{ pulses} \times (\pi/3 \text{ cm/pulse}) = 10\pi/3 \text{ cm} \approx 10.47 \text{ cm}$

    * **회전 각도($\theta$) 계산**:
        * 로봇이 회전할 때, 각 바퀴의 이동 거리 차이는 로봇의 회전 각도와 회전 반경에 따라 결정됩니다.
        * $\theta = (d_L - d_R) / L$
        * $\theta = (20\pi/3 - 10\pi/3) / 20 = (10\pi/3) / 20 = 10\pi/60 = \pi/6 \text{ radians}$
        * $\theta = (\pi/6) \times (180^\circ/\pi) = 30^\circ$

    * **회전 반경($R$) 계산**:
        * 로봇의 회전 중심으로부터 중앙까지의 거리 $R = L/2 \times (d_L + d_R) / (d_L - d_R)$
        * 또는, 더 간단하게 각 바퀴의 회전 반경을 이용:
            * $R_L = d_L / \theta = (20\pi/3) / (\pi/6) = 20\pi/3 \times 6/\pi = 40 \text{ cm}$ (왼쪽 바퀴의 회전 반경)
            * $R_R = d_R / \theta = (10\pi/3) / (\pi/6) = 10\pi/3 \times 6/\pi = 20 \text{ cm}$ (오른쪽 바퀴의 회전 반경)
        * 로봇의 중심이 이동한 회전 반경 $R_{center} = R_R + L/2 = 20 + 20/2 = 20 + 10 = 30 \text{ cm}$
        * 또는 $R_{center} = R_L - L/2 = 40 - 10 = 30 \text{ cm}$
        * 따라서, **로봇의 회전 반경(Radius of Rotation)은 약 $30 \text{ cm}$ 입니다.** (오른쪽으로 회전)

3.  **마지막 직진 구간 (Final Straight Driving Section)**:
    * 기간: 50 펄스(pulse)
    * 양쪽 바퀴 엔코더 측정값 동일.
    * 이동 거리: $50 \text{ pulses} \times (\pi/3 \text{ cm/pulse}) = 50\pi/3 \text{ cm} \approx 52.36 \text{ cm}$
    * 회전 각도: $0^\circ$ (직진 이동)

**총 이동 설명(Total Movement Description)**:
로봇은 먼저 약 **$104.72 \text{ cm}$를 직진**했습니다. 그 다음, 왼쪽 바퀴가 $20 \text{ pulses}$ 이동하는 동안 오른쪽 바퀴가 $10 \text{ pulses}$ 이동하여, **오른쪽으로 약 $30^\circ$ 회전**했습니다. 이 회전은 약 $30 \text{ cm}$의 회전 반경(Radius of Rotation)으로 이루어졌습니다. 마지막으로, 로봇은 다시 약 **$52.36 \text{ cm}$를 직진**했습니다.

---