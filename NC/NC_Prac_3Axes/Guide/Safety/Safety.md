## 목차
- [개요](#개요)
- [개발환경](#개발환경)
- [Safety 프로젝트 생성](#Safety-프로젝트-생성)
  - [프로젝트 생성](#프로젝트-생성)
  - [Target System 설정](#Target-System-설정)
  - [Alias Devices 설정](#Alias-Devices-설정)
  - [Safety Application Language 설정](#Safety-Application-Language-설정)
  - [GVL 생성 및 변수 Link](#GVL-생성-및-변수-Link)
  - [프로젝트 Verify 및 Activate](#프로젝트-Verify-및-Activate)
- [테스트](#테스트)
- [참조](#참조)

## 개요
기초적인 Safety 프로젝트를 생성하는 방법을 설명합니다.   
[이전 Guide 글](https://github.com/daegunsoftDev/PLC-Samples/tree/main/NC/NC_Prac_3Axes/Guide/PLC)에서 이어서 설명합니다.

## 개발환경
- [TwinCAT 3 (3.1.4024.12)](https://drive.google.com/file/d/1ZhhuMs2BZOonUoDA96CvrTefrvMXhnZP/view?usp=sharing)    

## Safety 프로젝트 생성

### 프로젝트 생성
SAFETY를 선택하고 새 프로젝트를 생성합니다.   
목록에서 TwinCAT Safety Project Preconfigured Inputs를 선택합니다.

<img src="1 Safety Project 추가.png" width="90%">

### Target System 설정
Safety Project의 Target System을 선택합니다.   
Physical Device를 **EL6900**으로 설정하고 **Safe Address**를 **Hardware Address**와 동일하게 설정합니다.

<img src="2 Target System 설정.png" width="50%">

- Hardware Address는 모듈 측면의 DIP 스위치를 통해 설정하며 중복될 수 없습니다.

### Alias Devices 설정
1. Alias Devices에 **1 Digital Output (Standard) 3개**를 추가합니다.   
항목명은 각각 **FbErr, ComErr, Output** 으로 설정합니다.

2. Alias Device의 Context menu에서 **Import Alias-Device(s) from I/O-configuration**을 선택합니다.   
    **EL1904**와 **EL2904**를 선택하고 OK 버튼을 누릅니다.

  위의 과정을 완료한 결과는 아래 사진과 같습니다.

  <img src="3 Alias Devices 구성.png" width="40%">

### Safety Application Language 설정
1. TwinSafeGroup1.sal을 선택하고 Toolbox에서 safeOr를 선택하여 Network1에 추가합니다.   
그리고 Variable Mapping - Variables에 변수 4개를 추가합니다.

2. Variable1의 Assignment를 EL1904의 **InputChannel1**, Usages를 FBAnd1의 **OrIn1** 으로 설정합니다.   
Variable2의 Assignment를 FbAnd1의 **OrOut**, Usages를 EL2904의 **OutputChannel1**, Alias Devices의 **Output.Out** 으로 설정합니다.   
Variable3의 Assignment를 TwinSafeGroup1의 **Fb Err**, Usages를 Alias Devices의 **FbErr.Out** 으로 설정합니다.   
Variable4의 Assignment를 TwinSafeGroup1의 **Com Err**, Usages를 Alias Devices의 **ComErr.Out** 으로 설정합니다.   

### GVL 생성 및 변수 Link
1. PLC 프로젝트에 GVL을 생성합니다.

```
VAR_GLOBAL
	bSafetyRun AT %Q*	:	BOOL;
	bSafetyErrorAck AT %Q*	:	BOOL;
	bSafetyComErr AT %I*	:	BOOL;
	bSafetyFbErr AT %I*	:	BOOL;
	bSafetyOutput AT %I*	:	BOOL;
END_VAR
```

2. GVL의 변수들을 Safety - Alias Devices의 항목들과 Link합니다.

### 프로젝트 Verify 및 Activate
1. TwinCAT Safety Toolbar 에서 **Verify Safety Project** 버튼을 누릅니다.   
Error List를 확인하여 문제가 없다면 **Verify Complete Safety Project** 버튼을 누릅니다.   
Error List를 확인하여 문제가 없다면 **Download Safety Project** 버튼을 누릅니다.

2. 로그인 계정과 비밀번호의 기본값은 Administrator / TwinSAFE 입니다.

## 테스트
1. SAL Worksheet를 선택한 상태에서 TwinCAT Safety Toolbar의 **Show Online Data**를 누르면 Group의 상태와 Online Value를 확인할 수 있습니다.    
SafeGroup의 상태가 Run 상태여야 Safety Logic이 작동합니다.

2. Target System에 로그인하고 PLC 프로그램을 실행합니다.   

3. GVL의 bSafetyRun을 TRUE로 설정합니다.   
그 다음 bSafetyErrorAck를 TRUE로 설정하고 다시 FALSE로 설정합니다.   
bSafetyComErr가 FALSE가 되고 Group 상태가 Run으로 변경됩니다.   

4. Safety In_00 토글 스위치를 조작하여 Safety Logic이 정상적으로 동작하는지 확인합니다.   

## 참조

[Beckhoff Infosys: Safety](https://infosys.beckhoff.com/english.php?content=../content/1033/tc3_safety_intro/html/tc3_safety_intro.htm&id=4314916724452125872)