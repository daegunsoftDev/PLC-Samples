## 목차
- [개요](#개요)
- [개발환경](#개발환경)
- [프로젝트 구현](#프로젝트-구현)
  - [I/O 설정 및 모터 테스트](#I/O-설정-및-모터-테스트)
  - [코드 작성 및 테스트](#코드-작성-및-테스트)
- [참조](#참조)

## 개요
실습용 시스템의 입력 단자를 사용하여 각각의 모터를 제어하는 PLC 프로젝트를 구현합니다.

## 개발환경
- [TwinCAT 3 (3.1.4024.12)](https://drive.google.com/file/d/1ZhhuMs2BZOonUoDA96CvrTefrvMXhnZP/view?usp=sharing)    

## 프로젝트 구현

### I/O 설정 및 모터 테스트
1. I/O - Devices에서 Scan을 실행합니다.   
   실습용 시스템에 AX5203과 EL7201이 설치되어 있으므로 아래와 같은 대화상자가 표시됩니다.

   <img src="1 Scan Motors 대화상자.png" width="40%">

   모터 종류를 수동으로 설정하는 방법을 설명하기 위해 아니요를 선택합니다.   
   새로 추가되는 Axis의 종류는 NC를 선택합니다.   

   <img src="2 Axis 추가 대화상자.png" width="40%">

2. I/O에서 **AX5203-0000-0203**을 선택합니다.   
   **AX5203-0000-0203**의 Channel A에는 **AM8022-0D20-0000**이 연결되어 있습니다.   
   Drive Manager의 Channel A - Configuration - Motor and Feedback에서 Select motor 버튼을 클릭하여 모터 종류를 지정합니다.   
   그 다음 Scaling and NC parameters에서 Set NC Parameters 버튼을 클릭하여 Parameter 값을 변경합니다.   

   <img src="3 Motor 종류 설정.gif" width="80%">

   Channel B에는 **AM8022-0D00-0000**이 연결되어 있습니다. Channel A와 같은 방법으로 설정합니다.

3. I/O에서 **EL7201**을 선택하고 모터 종류를 **AM8122-0F00-0000**로 설정합니다.   
   (EL7201와 호환되지 않는 모터이므로 Force 버튼을 눌러 진행합니다.)   

   <img src="4 Motor Force Select.png" width="40%">

4. Activate Configuration 버튼을 눌러 변경사항을 적용하고 모터가 정상적으로 움직이는지 테스트합니다.

### 코드 작성 및 테스트
   이번 예제에서는 실습용 시스템의 노브(**AI_00**)와 토글 스위치(**DI_00 ~ DI_02**)를 사용하여 모터를 제어합니다.   
   Axis를 객체로 다루기 위해 [PLC-NC-Package](https://github.com/daegunsoftDev/PLC-NC-Package) 프로젝트의 Axis 관련 코드를 사용했습니다.   

   모터의 동작은 **DI_00 ~ DI_02** 토글 스위치를 통해 제어됩니다.   
   모터의 속도는 **AI_00** 노브를 통해 제어됩니다. 속도를 변경하기 위해서 우선 모터를 정지시킨 후 다시 동작시켜야 합니다.

## 참조

* [NC Errors | TwinCAT 3](https://infosys.beckhoff.com/content/1033/tc3ncerrcode/index.html)
