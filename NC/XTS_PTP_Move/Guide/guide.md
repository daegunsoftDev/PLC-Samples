## 목차
- [개요](#개요)
- [프로젝트 구현](#프로젝트-구현)
  - [XTS 설치](#XTS-설치)
  - [라이브러리 추가하기](#라이브러리-추가하기)
  - [XTS_Task 추가 및 코어 설정](#XTS_Task-추가-및-코어-설정)
  - [XTS Module 추가](#XTS-Module-추가)
  - [XTS IO Driver 추가](#XTS-IO-Driver-추가)
  - [MOTION Parameter 설정](#MOTION-Parameter-설정)
  - [코드 작성 및 PLC 인스턴스 변수 Link](#코드-작성-및-PLC-인스턴스-변수-Link)
- [테스트](#테스트)
  - [TcXtsViewer 실행](#TcXtsViewer-실행)
  - [PLC 실행 방법](#PLC-실행-방법)

## 개요
가상의 XTS 시스템을 구성하고 간단한 PTP 제어가 가능한 프로그램을 작성합니다.

## 프로젝트 구현

### XTS 설치
[TF5850(3.20.705.0)](https://drive.google.com/file/d/13jeuTTDplzaEVIwHbdIx4sN3SKehuBl2/view?usp=sharing)   
[TC3 Advanced Motion Pack(3.1.10.14)](https://drive.google.com/file/d/1KkWCg5_1TUxK2w9vqB9zu4BEDfKI_Cwq/view?usp=sharing)   
위의 두 개 항목들을 다운로드 받은 후 Target System에 설치합니다.

### 라이브러리 추가하기
PLC 프로젝트의 References에 Tc2_MC2, Tc3_McCoordinatedMotion, Tc3_McCollisionAvoidance 라이브러리를 추가합니다.

### XTS_Task 추가 및 코어 설정
XTS를 제어하려면 하나의 Isolated Core를 독점적으로 사용하는 Task가 필요합니다.   
Solution Explorer의 SYSTEM - Tasks에서 'XTS_Task' Task를 추가합니다.   
<img src="1 XTS_Task 추가.gif" width="50%">

이제 Core 설정을 수정합니다.   
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
<img src="2 Core 설정 수정.gif" width="50%">

마지막으로 다시 SYSTEM - Tasks로 이동해서 XTS_Task의 Cycle ticks를 1로 수정합니다.

### XTS Module 추가
가상의 XTS 모듈들을 추가하여 가상의 XTS 시스템을 구성합니다.   
I/O - Devices의 Context Menu에서 Add New Item...을 선택하고 대화상자에서 EtherCAT - EtherCAT Master를 선택합니다.   
<img src="3 Device 추가.png" width="40%">   
<img src="4 EtherCAT Master 추가.png" width="40%">

새로 추가된 EtherCAT Master에 XTS - AT2001-0250 Motor module with feed 250mm, 48V를 추가합니다.   
<img src="5 XTS 전원 모듈 추가.png" width="40%">   
이 모듈은 전원선이 연결되며 일반적으로 무버들의 원점으로 사용됩니다.   
방금 추가한 AT2001-0250을 선택하고 AT2001-5250 Sensor line with feed를 같은 방법으로 추가합니다.

이번 예제에서는 총 길이 3000mm의 트랙을 구성할 것입니다.   
전체 트랙은 직선 모듈 8개와 90° clothoid 모듈 4개로 구성됩니다.   
<img src="6 XTS 트랙 구성도.png" width="40%">

직선 모듈 3개를 더 추가해야 합니다. EtherCAT Master에 AT2000-0250 모듈 3개를 추가합니다.   
각 모듈에는 AT2000-5250 Sensor Line 모듈을 추가합니다.   
곡선 모듈은 AT2050-0500 Motor module, 90° clothoid, EtherCAT In, 48V를 먼저 추가합니다.
그 다음 AT2050-0501 모듈을 추가합니다.   
마찬가지로 두 모듈에 적절한 Sensor Line 모듈을 추가합니다.

트랙의 절반을 완성했습니다. I/O - Devices에 EtherCAT Master를 하나 더 추가하고 같은 과정을 반복합니다.   
마지막으로 두 EtherCAT Master는 실제로 존재하는 장치가 아니므로 Context Menu에서 Disable을 클릭하여 비활성화 합니다.

### XTS IO Driver 추가
SYSTEM - TcCOM Objects에 XtsIoDriver를 추가합니다.   
<img src="7 XtsIoDriver 추가.png" width="40%">   

XtsIoDriver를 선택하고 Manager - Launch Configurator 버튼을 클릭합니다.   
XTS 구성을 진행하는 창이 표시됩니다. Task는 XTS_Task를 선택합니다.   
Device 선택 단계에서 각 Device를 순서에 따라 선택합니다.   
트랙이 올바르게 구성되었는지 확인하고 다음으로 넘어갑니다.   
이 예제에서는 2개의 무버를 사용합니다.   
무버 추가 후 다음 버튼을 두번 눌러 설정을 완료합니다.   
<img src="8 XtsIoDriver Manager 초기 설정.gif" width="40%">   

가상의 XTS 장치를 사용할 것이므로 가상의 무버들의 초기 위치를 설정해줘야 합니다.   
Launch Configurator 버튼을 다시 클릭하고 아래 이미지를 참조하여 설정을 완료합니다.   
<img src="9 XtsIoDriver Manager 시뮬레이션 설정.gif" width="40%">   

### MOTION Parameter 설정
MOTION - NC-Task - Objects - Object1 (CA Group) 을 선택합니다.   
Parameter (Init) 탭의 Rail Length가 XTS 시스템의 트랙 길이와 동일한지 확인합니다.   
이 예제에서 구성한 XTS 시스템의 트랙 길이는 3000mm입니다.   

그 다음으로 Axes - Axis 1 - Enc 를 선택합니다.   
Parameter 탭의 Modulo Factor가 Rail Length와 동일하도록 수정합니다.   

### 코드 작성 및 PLC 인스턴스 변수 Link
코드 본문이 너무 길어서 여기에 적지 않습니다.   
코드 작성 후 PLC 인스턴스 변수들을 각각의 Axis와 CA Group에 Link 합니다.   

## 테스트

### TcXtsViewer 실행
기본 설치 경로는 C:\TwinCAT\Functions\TF5850-TC3-XTS-Technology\TcXtsViewer\TcXtsViewer.exe 입니다.   
<img src="10 TcXtsViewer 실행.gif" width="40%">   

### PLC 실행 방법
1. bEnable을 TRUE로 설정하여 Axis들을 동작 가능한 상태로 만듭니다.
2. bAddToGroup을 TRUE로 설정하여 Axis들을 CA Group에 추가합니다.
3. bGroupEnable을 TRUE로 설정하여 CA Group을 동작 가능한 상태로 만듭니다.
4. fPosition2, fVelocity2, fGap2를 각각 200, 20, 100으로 설정합니다.
5. bMoveExecute2를 TRUE로 설정하면 Mover 2가 지정된 동작대로 이동합니다.
