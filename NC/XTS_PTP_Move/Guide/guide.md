## 목차
- [개요](#개요)
- [개발환경](#개발환경)
- [프로젝트 구현](#프로젝트-구현)
  - [XTS 설치](#XTS-설치)
  - [라이브러리 추가하기](#라이브러리-추가하기)
  - [XTS Module 추가](#XTS-Module-추가)
  - [XTS Tool을 사용하여 XTS 시스템 구성](#XTS-Tool을-사용하여-XTS-시스템-구성)
  - [XTS_Task 설정](#XTS-Task-설정)
  - [기타 Parameter 설정](#기타-Parameter-설정)
  - [코드 작성 및 PLC 인스턴스 변수 Link](#코드-작성-및-PLC-인스턴스-변수-Link)
- [테스트](#테스트)
  - [TcXtsViewer 실행](#TcXtsViewer-실행)
  - [PLC 실행 방법](#PLC-실행-방법)
- [참조](#참조)

## 개요
  가상의 XTS 시스템을 구성하고 간단한 PTP 제어가 가능한 프로그램을 작성합니다.

## 개발환경
- TwinCAT 3 (v3.1.4024.12)   
- TF5850 (3.20.705.0)   
- TC3 Advanced Motion Pack (3.1.10.14)

## 프로젝트 구현

### XTS 설치
- [TwinCAT 3 (3.1.4024.12)](https://drive.google.com/file/d/1ZhhuMs2BZOonUoDA96CvrTefrvMXhnZP/view?usp=sharing)   
- [TF5850 (3.20.705.0)](https://drive.google.com/file/d/1kwbirsjRMprR-XAxFGDyAby39jpsAurl/view?usp=sharing)   
- [TC3 Advanced Motion Pack (3.1.10.14)](https://drive.google.com/file/d/1bPDNav58DQjhoBK_NNgb-H1zfb68s3sS/view?usp=sharing)   

  위의 3개 항목들을 설치합니다.   
  (**Target System**이 따로 있는 경우 해당 시스템에도 같이 설치합니다.)

### 라이브러리 추가하기
  PLC 프로젝트의 References에 **Tc2_MC2, Tc3_McCoordinatedMotion, Tc3_McCollisionAvoidance** 라이브러리를 추가합니다.

### XTS Module 추가

가상의 XTS 모듈들을 추가하여 가상의 XTS 시스템을 구성합니다.   
**I/O - Devices**의 Context Menu에서 **Add New Item...**을 선택하고 대화상자에서 EtherCAT - **EtherCAT Master**를 선택합니다.   
<img src="1 Device 추가.png" width="40%"><img src="2 EtherCAT Master 추가.png" width="50%">

새로 추가된 EtherCAT Master에 XTS - **AT2001-0250 Motor module with feed 250mm, 48V**를 추가합니다.   
<img src="3 XTS 전원 모듈 추가.png" width="40%">   
이 모듈은 전원선이 연결되며 일반적으로 무버들의 원점으로 사용됩니다.   
방금 추가한 **AT2001-0250**을 선택하고 **AT2001-5250 Sensor line with feed**를 같은 방법으로 추가합니다.

이번 예제에서는 총 길이 **3000mm**의 트랙을 구성할 것입니다.   
전체 트랙은 **직선 모듈 8개**와 **90° clothoid 모듈 4개**로 구성됩니다.   
<img src="4 XTS 트랙 구성도.png" width="40%">

직선 모듈 3개를 더 추가해야 합니다. EtherCAT Master에 **AT2000-0250** 모듈 3개를 추가합니다.   
각 모듈에는 **AT2000-5250 Sensor Line** 모듈을 추가합니다.   
곡선 모듈은 **AT2050-0500 Motor module, 90° clothoid, EtherCAT In, 48V**를 먼저 추가합니다.
그 다음 **AT2050-0501** 모듈을 추가합니다.   
마찬가지로 두 모듈에 적절한 Sensor Line 모듈을 추가합니다.

트랙의 절반을 완성했습니다. I/O - Devices에 EtherCAT Master를 하나 더 추가하고 같은 과정을 반복합니다.   
마지막으로 두 **EtherCAT Master**는 실제로 존재하는 장치가 아니므로 Context Menu에서 **Disable을 클릭하여 비활성화** 합니다.

### XTS Tool을 사용하여 XTS 시스템 구성

IDE의 Menu bar에서 View - Other Windows - XTS Tool Window를 선택합니다.
<img src="5 XTS Tool 실행.gif" width="50%">

XTS Tool Window가 열립니다. 상단의 <img src="6 XTS Configurator 아이콘.png"> 아이콘을 클릭하여 XTS Configurator를 실행합니다.   
<img src="7 XTS 설정 과정 Processing Units.gif" width="50%">   
XTS Processing Unit은 XTS 시스템을 제어하는 가장 큰 단위이며 여러개의 Task를 가질 수 있습니다.   
이번 예제에서는 1개의 XTS Task만 사용합니다. Task를 생성하고 다음 과정을 진행합니다.

<img src="8 XTS 설정 과정 Parts.gif" width="50%">   
XTS Part에는 하나 이상의 Feed Line이 포함됩니다.   
1개의 XTS Part를 생성하고 Device 1, Device 2 순서로 추가합니다.   

<img src="9 XTS 설정 과정 Tracks.gif" width="50%">   

XTS Track은 하나 이상의 XTS Part로 구성됩니다.   
1개의 XTS Track을 생성하고 이전 단계에서 생성한 Part를 추가합니다.   

다음 과정인 XTS Station은 이번 예제에서 다루지 않습니다.   
그 다음 과정인 Mover로 바로 진행합니다.   
<img src="10 XTS 설정 과정 Movers.png" width="50%">   
2개의 Mover를 추가합니다. 다음으로 진행하여 설정을 완료합니다.

### XTS_Task 설정

XTS를 제어하려면 하나의 Isolated Core를 독점적으로 사용하는 Task가 필요합니다.   
직전 과정에서 추가한 XTS Task를 적절하게 수정해야 합니다.

SYSTEM - Real-Time을 선택합니다.   
Settings 탭의 'Read from Target' 버튼을 클릭하면 현재 선택된 Target System의 코어 구성을 확인할 수 있습니다.

* Target System이 일반 PC인 경우:   
    XTS_Task는 Shared Core에 할당할 수 없고 Isolated Core에 할당되어야 합니다.   
    Isolated Core가 없으면 'Set on target' 버튼을 눌러 두 종류의 코어의 개수를 조절합니다.   
    이번 예제에서는 2개의 Isolated Core를 사용합니다.   
    Isolated로 설정된 Core는 윈도우에서 사용할 수 없게 되므로 필요한 만큼만 설정합니다.   
    설정을 적용하기 위해 재부팅 하고 'Read from Target' 버튼을 다시 누릅니다.   
    목록 하단의 2개 core가 Isolated로 변경된 것을 확인하고 RT_Core 체크박스에 체크합니다.   
    목록 상단의 0번 Core는 체크 해제합니다.   

XTS_Task는 다른 Task들과 다른 Core에 할당하고, 해당 코어의 Base Time을 250μs로 수정합니다.   
전체 과정은 아래와 같습니다.   
<img src="11 Core 설정 수정.gif" width="50%">

이후 SYSTEM - Tasks 에서 XTS_Task의 Cycle ticks를 1로 수정합니다.

### 기타 Parameter 설정
SYSTEM - TcCOM Objects - XtsProcessingUnit을 선택합니다.   
Parameter (Init)의 General 에서 OperationMode를 Simulation으로 변경합니다.

MOTION - NC-Task - Objects에 CA Group[Module]을 추가합니다.   
CA Group의 Parameter (Init) 에서 Geometry의 Rail Length가 XTS 시스템의 트랙 길이(3000mm)와 동일한지 확인합니다.   
Track이 폐곡선 형태이므로 Rail is Ring 속성은 TRUE로 설정되어야 합니다.   

그 다음으로 Axes - Axis 1 - Enc 를 선택합니다.   
Parameter 탭의 Modulo Factor가 Rail Length와 동일하도록 수정합니다.   
Axis 2도 같은 방법으로 확인합니다.

### 코드 작성 및 PLC 인스턴스 변수 Link
코드 본문이 너무 길어서 여기에 적지 않습니다.   
코드 작성 후 PLC 인스턴스 변수들을 각각의 Axis와 CA Group에 Link 합니다.   

## 테스트

### TcXtsViewer 실행
기본 설치 경로는 C:\TwinCAT\Functions\TF5850-TC3-XTS-Technology\TcXtsViewer\TcXtsViewer.exe 입니다.   
<img src="12 TcXtsViewer 실행.gif" width="40%">   

### PLC 실행 방법
1. bEnable을 TRUE로 설정하여 Axis들을 동작 가능한 상태로 만듭니다.
2. bAddToGroup을 TRUE로 설정하여 Axis들을 CA Group에 추가합니다.
3. bGroupEnable을 TRUE로 설정하여 CA Group을 동작 가능한 상태로 만듭니다.
4. fPosition2, fVelocity2, fGap2를 각각 200, 20, 100으로 설정합니다.
5. bMoveExecute2를 TRUE로 설정하면 Mover 2가 지정된 동작대로 이동합니다.

## 참조
- [Infosys: TF5410 | TwinCAT 3 Motion Collision Avoidance](https://infosys.beckhoff.com/english.php?content=../content/1033/tf5410_tc3_collision_avoidance/index.html&id=2112423517878358696)
- [Infosys: TF5850 | TC3 XTS Extension(German)](https://infosys.beckhoff.de/index.php?content=../content/1031/xts_software/index.html&id=325510458286519059)
