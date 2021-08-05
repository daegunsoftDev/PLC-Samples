## 목차
- [개발환경](#개발환경)
- [라이브러리 추가하기](#라이브러리-추가하기)
- [NC Task 및 Axis 추가](#NC-Task-및-Axis-추가)
- [PLC 인스턴스에 AXIS_REF 변수 추가](#PLC-인스턴스에-AXIS_REF-변수-추가)
- [NC Axis를 PLC 인스턴스 변수에 Link](#NC-Axis를-PLC-인스턴스-변수에-Link)
- [코드 작성 및 실행](#코드-작성-및-실행)

## 개발환경
- [TwinCAT 3 (3.1.4024.12)](https://drive.google.com/file/d/1ZhhuMs2BZOonUoDA96CvrTefrvMXhnZP/view?usp=sharing)   

## 라이브러리 추가하기
솔루션 생성 및 PLC 프로젝트를 추가한 후, Motion 관련 기능을 사용하기 위해 Tc2_MC2 라이브러리를 추가해야 합니다.   

<img src="1 라이브러리 추가.gif" width="60%">

## NC Task 및 Axis 추가
실제 NC를 수행하는 NC Task와 제어할 Axis를 추가합니다.   

<img src="2 NC Task 및 Axis 추가.gif" width="60%">

## PLC 인스턴스에 AXIS_REF 변수 추가
MAIN 프로그램의 선언부를 아래와 같이 작성합니다.

```
PROGRAM MAIN
VAR
	Axis			:	AXIS_REF;
END_VAR
```
[AXIS_REF](https://infosys.beckhoff.com/content/1033/tcplclib_tc2_mc2/70132363.html?id=467387231256035047)는 NC와 PLC간의 인터페이스로 Axis에 대한 정보를 가지고 있습니다.   
작성 후 솔루션을 빌드하면 PLC 인스턴스에 변수가 추가됩니다.

<img src="3 솔루션 빌드.png" width="50%">
<img src="4 PLC 인스턴스에 변수 추가됨.png" width="50%">

## NC Axis를 PLC 인스턴스 변수에 Link
Axis를 PLC에서 제어하려면 Axis를 PLC 인스턴스의 변수와 Link 해야 합니다.   
Motion - NC-Task - Axes에서 Axis 선택 후 Settings 탭의 Link To PLC... 버튼을 누릅니다.   
대화상자에서 PLC 인스턴스의 변수를 선택합니다.   

<img src="5 Axis Link To PLC.gif" width="50%">

## 코드 작성 및 실행
MAIN 프로그램의 선언부를 아래와 같이 수정합니다.

```
PROGRAM MAIN
VAR
	Axis		:	AXIS_REF;

	fbPower		:	MC_Power;
	bEnable		:	BOOL;
	bIsAxisReady	:	BOOL;
	
	fbMoveAbsolute	:	MC_MoveAbsolute;
	bExecute	:	BOOL;
	fPosition	:	LREAL;
	fVelocity	:	LREAL;
	bBusy		:	BOOL;
	bDone		:	BOOL;
END_VAR
```

[MC_Power](https://infosys.beckhoff.com/english.php?content=../content/1033/tcplclib_tc2_mc2/70049419.html&id=3860055855707485697) FB는 컨트롤러의 Axis 제어 가능 여부를 설정합니다.   
[MC_MoveAbsolute](https://infosys.beckhoff.com/english.php?content=../content/1033/tcplclib_tc2_mc2/70094731.html&id=7626760230426344577) FB는 Axis를 지정한 위치까지 지정한 속도로 이동하게 합니다.   
각 FB에 대한 자세한 설명은 링크를 참조하시기 바랍니다.

MAIN 프로그램의 구현부를 아래와 같이 작성합니다.

```
Axis(); // Axis 정보 갱신

fbPower(
	Axis := Axis,
	Enable := bEnable,
	Enable_Positive := bEnable,
	Enable_Negative := bEnable,
	Status => bIsAxisReady // Axis가 동작할 준비가 완료되면 TRUE.
);

fbMoveAbsolute(
	Axis := Axis,
	Execute := bExecute,
	Position := fPosition,
	Velocity := fVelocity,
	Busy => bBusy,	// 요청받은 동작을 처리중일 때 TRUE.
	Done => bDone 	// 요청받은 동작이 완료되면 TRUE.
);
```

코드 작성을 완료했다면 솔루션을 다시 빌드합니다. 코드에 문제가 없으면 Activation을 진행합니다.   

Axis를 움직이려면 먼저 Controller를 활성화 해야 합니다. bEnable 변수를 True로 설정합니다.   
Axis가 동작할 준비가 완료되면 fbPower의 Output인 Status 및 bIsAxisReady 변수에 True가 출력됩니다.   

fPosition과 fVelocity에 적절한 값(ex. 200, 10)을 입력하고 bExecute를 True로 설정하면 Axis가 입력받은 위치와 속도로 이동합니다.
