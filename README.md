# 차선 주행 및 아르코마커 인식
디지털 트윈 기반 서비스 로봇 운영 시스템 구성(real world with waffle)

<br>

### Contributors
|정서윤|홍진규|나승원|이동기|
|----|----|----|----|
|**[???](https://github.com/)**|**[???](https://github.com/)**|**[lala4768](https://github.com/lala4768)**|**[???](https://github.com/)**|

<br>

## 프로젝트 개요

* **주제**
  
   차선 주행 및 아르코마커 인식 
* **개발 배경**

  * 시뮬레이션에서 구현된 로봇을 real world로 옮겨와 동일한 task 수행을 가능케 하고자 했음
  * 이번 프로젝트에서 배운 aruco marker 인식 기능을 추가해 보다 다양한 역할을 수행할 수 있도록 하고자 했음
* **목표**

  * 시뮬레이션 코드를 기반으로 real world에서 차선 탐지 후 주행 + 횡단보도 인식 기능 구현
  * Aruco marker를 인식하면 manipulator가 이를 장애물로 판단해 도로 외곽으로 옮기는 기능 추가 구현
<br>

# Flow Chart
`차선 감지 및 주행` → `Aruco Marker 검출시 정지` → `Pick and Place`

<br>

## 사용 기술 및 장비

* **언어 및 환경**: Python 3.10, Ubuntu 22.04, ROS2 Humble
* **도구**: VSCode
* **로봇 하드웨어**: TurtleBot3, OpenMANIPULATOR
* **로봇 소프트웨어**: rviz

<br>

## 주요 기능

1. **차선 감지**

![lane_trace-ezgif com-video-to-gif-converter](https://github.com/user-attachments/assets/317a4965-f53e-4345-9b07-f067c66c0e61)

* 파라미터 선언 & 동적 로딩
* 상태 변수 초기화
* 토픽-구독-퍼블리셔-타이머 설정
* PID 파라미터 재정의
* 콜백 함수를 정의함으로써 최신 이미지와 관절 상태를 지속 업데이트해, 이후 제어 로직이 최신 데이터를 사용할 수 있도록 함 
* 주행 속도 및 회전 속도 명령을 간단히 보내기 위한 헬퍼 함수 정의
* 검사한 마커가 ID와 일치하면 플래그 세팅해 이미지 콜백 함수에서 플래그 확인 후 로봇 정지 및 plck & place 트리거 로직 실행

2. **Aruco Marker 검출**

![traffic_light-ezgif com-video-to-gif-converter](https://github.com/user-attachments/assets/79269e7d-15c2-4f49-83ff-a70b177c3a50)

* 신호 감지 노드를 실행하고, 색상 인식에 필요한 HSV 파라미터 설정
* 카메라로부터 실시간 이미지 구독하고 OpenCV로 빨간불/노란불/초록불 감지
* 신호동의 상태가 담긴 메시지를 토픽을 통해 전달
* 감지된 신호등의 상태에 따라 로봇의 주행 속도나 정지 여부 결정
* 제어 노드 실행해 실제 로봇 동작 제어


3. ** Pick and Place**

![parking-ezgif com-video-to-gif-converter (1)](https://github.com/user-attachments/assets/0ec9c6ab-bcdc-4db8-98d9-c6089fbc0fa5)


* 정지 실행 -> 빈 메시지를 퍼블리시해 로봇의 선속도와 회전속도를 0으로 만들어 즉시 정지
* 로그 기록 -> "Stopped after delay" 로그 남겨 정지 지점 확인
* 타이머 정리 -> 이전에 설정된 지연 정지용 타이머를 더이상 호출되지 않게 하고, 초기화해 중복 실행 방지
* 

<br>

## 프로젝트 성과

* Simulation 환경에서 개발한 코드를 기반으로 Real World에서 주어진 Task 수행 완료
* TurtleBot3와 Manipulator를 활용한 차선 주행 및 Pick and Place Task를 서로 연계
* ROS2 기반 통신 구조 설계 및 노드 연동 경험

![simulation-ezgif com-video-to-gif-converter](https://github.com/user-attachments/assets/8ad4640b-ad9b-459d-8325-705885b77f47)
