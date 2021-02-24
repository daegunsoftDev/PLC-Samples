## 목차
- [라이브러리 추가하기](#라이브러리-추가하기)
- [NC Task 및 Axis 추가](#NC-Task-및-Axis-추가)
- [PLC 인스턴스에 AXIS_REF 변수 추가](#PLC-인스턴스에-AXIS_REF-변수-추가)
- [NC Axis를 PLC 인스턴스 변수에 Link](#NC-Axis를-PLC-인스턴스-변수에-Link)
## 라이브러리 추가하기
솔루션 생성 및 PLC 프로젝트를 추가한 후, Motion 관련 기능을 사용하기 위해 Tc2_MC2 라이브러리를 추가해야 합니다.   

<img src="https://user-images.githubusercontent.com/79190459/108954617-6eca3780-76b0-11eb-8d34-3eaabd48d995.gif" width="60%">

## NC Task 및 Axis 추가
실제 NC를 수행하는 NC Task와 제어할 Axis를 추가합니다.   

<img src="https://user-images.githubusercontent.com/79190459/108954626-712c9180-76b0-11eb-9e67-008def229245.gif" width="60%">

## PLC 인스턴스에 AXIS_REF 변수 추가
MAIN 프로그램의 선언부를 아래와 같이 작성합니다.

```
PROGRAM MAIN
VAR
	Axis			:	AXIS_REF;
END_VAR
```
AXIS_REF는 NC와 PLC간의 인터페이스로 Axis에 대한 정보를 가지고 있습니다.   
작성 후 솔루션을 빌드하면 PLC 인스턴스에 변수가 추가됩니다.

<img src="https://user-images.githubusercontent.com/79190459/108955617-d5038a00-76b1-11eb-8894-61ac738575f8.png" width="50%">
<img src="https://user-images.githubusercontent.com/79190459/108955925-480d0080-76b2-11eb-86fa-3644873b5f76.png" width="50%">

## NC Axis를 PLC 인스턴스 변수에 Link
Axis를 PLC에서 제어하려면 Axis를 PLC 인스턴스의 변수와 Link 해야 합니다.   
Motion - NC-Task - Axes에서 Axis 선택 후 Settings 탭의 Link To PLC... 버튼을 누릅니다.   
대화상자에서 PLC 인스턴스의 변수를 선택합니다.   

<img src="https://user-images.githubusercontent.com/79190459/108959035-29f5cf00-76b7-11eb-94c5-e288ae80a77f.gif" width="50%">

## 코드 작성 및 실행
MAIN 프로그램의 선언부를 아래와 같이 수정합니다.

```
PROGRAM MAIN
VAR
	Axis			:	AXIS_REF;

	fbPower			:	MC_Power;
	bEnable			:	BOOL;
	
	fbMoveAbsolute	:	MC_MoveAbsolute;
	bExecute		:	BOOL;
	fPosition		:	LREAL;
	fVelocity		:	LREAL;
END_VAR
```
