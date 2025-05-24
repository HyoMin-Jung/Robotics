# Differential Drive (차동 구동 방식) 로봇

## 1. 서론

이 문서는 두 개의 독립적인 바퀴 구동을 통해 이동 및 방향 전환을 수행하는 Differential Drive (차동 구동 방식)에 대해 설명합니다. Differential Drive (차동 구동 방식)는 로봇 공학 분야에서 모바일 로봇의 기본적인 이동 메커니즘으로 널리 사용됩니다. 이 보고서에서는 Differential Drive (차동 구동 방식)의 기본 개념, 방향 전환 원리, 관련 물리량 및 Turning Radius (회전 반경) 계산 방법에 대해 상세히 다룹니다.

## 2. Radian (라디안)

각도를 측정하는 단위에는 여러 가지가 있지만, 로봇의 회전과 관련된 계산에서는 Radian (라디안) 단위가 주로 사용됩니다.

### 2.1 Radian (라디안)의 정의

1 Radian (라디안)은 원의 반지름과 같은 길이의 호가 원의 중심에 이루는 각도를 의미합니다. 원 둘레 전체는 $2\pi$ Radian (라디안)이며, 이는 360도에 해당합니다.

### 2.2 호환 단위 및 변환

Radian (라디안)과 일반적으로 사용되는 각도 단위인 도(Degree)는 상호 변환이 가능합니다.

* **도(Degree)를 Radian (라디안)으로 변환:**
    각도(Radian) = 각도(도) $\times \frac{\pi}{180^\circ}$

* **Radian (라디안)을 도(Degree)로 변환:**
    각도(도) = 각도(Radian) $\times \frac{180^\circ}{\pi}$

예를 들어, 90도를 Radian (라디안)으로 변환하면 $90^\circ \times \frac{\pi}{180^\circ} = \frac{\pi}{2}$ Radian (라디안)이 됩니다. 반대로 $\pi$ Radian (라디안)을 도로 변환하면 $\pi \times \frac{180^\circ}{\pi} = 180^\circ$ 가 됩니다.

## 3. Differential Drive (차동 구동 방식)의 개념

### 3.1 정의 및 기본 원리

Differential Drive (차동 구동 방식)는 로봇 본체의 좌우에 각각 독립적으로 구동되는 두 개의 바퀴를 이용하여 이동하는 방식입니다. 각 바퀴의 속도를 개별적으로 제어함으로써 로봇의 직진 및 회전 운동을 구현합니다. 일반적으로 로봇의 균형을 위해 하나 이상의 캐스터 휠 또는 지지대가 추가될 수 있지만, 이동과 방향 전환의 주된 역할은 두 개의 구동 바퀴가 담당합니다.

기본 원리는 간단합니다. 두 바퀴가 같은 속도와 같은 방향으로 회전하면 로봇은 직진합니다. 두 바퀴의 속도에 차이를 두면 로봇은 속도가 느린 바퀴 쪽으로 회전하게 됩니다.

## 4. Angular Velocity (각속도)와 Turning Radius (회전 반경)의 개념

### 4.1 Angular Velocity (각속도)

Angular Velocity (각속도)는 물체가 회전할 때 단위 시간당 회전한 각도를 나타냅니다. 일반적으로 Radian/초 (rad/s) 단위를 사용합니다. 로봇의 바퀴에서는 바퀴가 회전하는 속도를 나타내며, 이는 로봇의 이동 속도와 회전 운동에 직접적인 영향을 미칩니다.

### 4.2 Turning Radius (회전 반경)

Turning Radius (회전 반경)는 로봇이 회전할 때 그리는 원호의 반지름을 의미합니다. Differential Drive (차동 구동 방식) 로봇은 두 바퀴의 속도 차이에 따라 다양한 크기의 Turning Radius (회전 반경)를 그리며 회전할 수 있습니다. 직진하는 경우는 Turning Radius (회전 반경)가 무한대인 특별한 경우로 볼 수 있습니다.

## 5. 각 바퀴의 Angular Velocity (각속도)가 이동 경로에 미치는 영향

Differential Drive (차동 구동 방식) 로봇의 이동 경로는 전적으로 좌우 바퀴의 Angular Velocity (각속도)에 의해 결정됩니다.

* **왼쪽 바퀴 Angular Velocity ($\omega_L$) = 오른쪽 바퀴 Angular Velocity ($\omega_R$) > 0:** 로봇은 앞으로 직진합니다. 이동 속도는 $(\omega_L + \omega_R) \times \frac{r}{2}$ 이고, Turning Radius (회전 반경)는 무한대입니다. 여기서 $r$은 바퀴의 반지름입니다.

* **왼쪽 바퀴 Angular Velocity ($\omega_L$) = 오른쪽 바퀴 Angular Velocity ($\omega_R$) < 0:** 로봇은 뒤로 직진합니다. 이동 속도는 $|(\omega_L + \omega_R) \times \frac{r}{2}|$ 이고, Turning Radius (회전 반경)는 무한대입니다.

* **왼쪽 바퀴 Angular Velocity ($\omega_L$) > 오른쪽 바퀴 Angular Velocity ($\omega_R$):** 로봇은 오른쪽 바퀴를 중심으로 왼쪽으로 회전합니다. Turning Radius (회전 반경)는 두 바퀴의 속도 차이에 따라 달라집니다.

* **왼쪽 바퀴 Angular Velocity ($\omega_L$) < 오른쪽 바퀴 Angular Velocity ($\omega_R$):** 로봇은 왼쪽 바퀴를 중심으로 오른쪽으로 회전합니다. Turning Radius (회전 반경)는 두 바퀴의 속도 차이에 따라 달라집니다.

* **왼쪽 바퀴 Angular Velocity ($\omega_L$) > 0, 오른쪽 바퀴 Angular Velocity ($\omega_R$) = 0:** 로봇은 오른쪽 바퀴를 중심으로 제자리에서 회전하지 않고, 오른쪽 바퀴를 축으로 왼쪽으로 원호를 그리며 이동합니다.

* **왼쪽 바퀴 Angular Velocity ($\omega_L$) = 0, 오른쪽 바퀴 Angular Velocity ($\omega_R$) > 0:** 로봇은 왼쪽 바퀴를 축으로 오른쪽으로 원호를 그리며 이동합니다.

## 6. 두 바퀴의 속도 차이에 따른 Turning Radius (회전 반경) 및 회전 방향의 변화

두 바퀴의 속도 차이가 커질수록 로봇의 Turning Radius (회전 반경)는 작아지고 더 빠르게 회전합니다. 반대로 속도 차이가 작아질수록 Turning Radius (회전 반경)는 커지고 더 완만하게 회전합니다.

회전 방향은 속도가 느린 바퀴 쪽으로 결정됩니다. 만약 왼쪽 바퀴의 속도가 오른쪽 바퀴의 속도보다 빠르면 로봇은 오른쪽 바퀴를 중심으로 왼쪽으로 회전하고, 오른쪽 바퀴의 속도가 왼쪽 바퀴의 속도보다 빠르면 로봇은 왼쪽 바퀴를 중심으로 오른쪽으로 회전합니다.

## 7. 제자리 회전 방법

Differential Drive (차동 구동 방식) 로봇이 제자리에서 회전(Zero-radius turn)하는 것은 두 바퀴의 속도를 같게 하고 방향을 반대로 하여 회전시킴으로써 가능합니다. 즉, 왼쪽 바퀴는 정방향으로 회전시키고 오른쪽 바퀴는 같은 속도로 역방향으로 회전시키면 로봇은 두 바퀴의 중간 지점을 중심으로 회전하게 됩니다. 반대로 왼쪽 바퀴를 역방향, 오른쪽 바퀴를 정방향으로 회전시키면 반대 방향으로 제자리 회전을 합니다.

## 8. 바퀴 간 거리 (Wheelbase)가 미치는 영향

바퀴 간 거리 (Wheelbase, $L$)는 로봇의 회전 성능에 중요한 영향을 미칩니다. Wheelbase (휠 베이스)가 길수록 동일한 바퀴 속도 차이에 대해 더 큰 Turning Radius (회전 반경)를 갖게 되어 선회 능력이 떨어질 수 있습니다. 반대로 Wheelbase (휠 베이스)가 짧을수록 더 작은 Turning Radius (회전 반경)로 빠르게 회전할 수 있지만, 직진 주행 시 조향이 불안정해질 수 있습니다.

## 9. Turning Radius (회전 반경)를 계산하는 방법

Differential Drive (차동 구동 방식) 로봇의 Turning Radius ($R$)는 좌우 바퀴의 선형 속도 ($v_L$, $v_R$)와 바퀴 간 거리 ($L$)를 이용하여 계산할 수 있습니다. 바퀴의 선형 속도는 Angular Velocity ($\omega$)와 바퀴의 반지름 ($r$)을 곱하여 얻을 수 있습니다 ($v = \omega \times r$).

로봇의 순간 회전 중심(Instantaneous Center of Curvature, ICC)은 두 바퀴의 연장선 상에 위치하며, ICC로부터 각 바퀴까지의 거리가 해당 바퀴의 회전 반경이 됩니다. 로봇의 중앙 지점부터 ICC까지의 거리가 로봇의 Turning Radius (회전 반경)로 간주됩니다.

로봇의 선형 속도 ($v$)와 Angular Velocity ($\omega$)는 다음과 같이 계산됩니다.
$v = \frac{v_R + v_L}{2} = \frac{(\omega_R + \omega_L)r}{2}$
$\omega = \frac{v_R - v_L}{L} = \frac{(\omega_R - \omega_L)r}{L}$

로봇의 Turning Radius ($R$)는 로봇의 선형 속도를 Angular Velocity (각속도)로 나눈 값으로 근사할 수 있습니다 (단, Angular Velocity (각속도)가 0이 아닐 때).
$R = \frac{v}{\omega} = \frac{(\omega_R + \omega_L)r / 2}{(\omega_R - \omega_L)r / L} = \frac{(\omega_R + \omega_L)L}{2(\omega_R - \omega_L)}$

만약 $\omega_R = \omega_L$, 즉 직진하는 경우 분모가 0이 되어 Turning Radius (회전 반경)는 무한대가 됩니다.
만약 $\omega_R = -\omega_L$이고 $\omega_L \neq 0$, 즉 제자리 회전하는 경우 분자가 0이 되어 Turning Radius (회전 반경)는 0이 됩니다.

이 공식은 로봇이 미끄러짐 없이 순수 구름 운동을 한다고 가정할 때 성립합니다. 실제 환경에서는 바퀴의 미끄러짐 등으로 인해 오차가 발생할 수 있습니다.