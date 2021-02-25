## 목차
- [라이브러리 추가하기](#라이브러리-추가하기)
- [XTS Module 추가](#XTS-Module-추가)
- [XTS IO Driver 추가](#XTS-IO-Driver-추가)
- [PLC 인스턴스에 AXIS_REF 변수 추가](#PLC-인스턴스에-AXIS_REF-변수-추가)
- [NC Axis를 PLC 인스턴스 변수에 Link](#NC-Axis를-PLC-인스턴스-변수에-Link)
- [코드 작성 및 실행](#코드-작성-및-실행)
- 
## 라이브러리 추가하기
References에 Tc2_MC2, Tc3_McCoordinatedMotion, Tc3_McCollisionAvoidance 라이브러리를 추가합니다.

## XTS Module 추가
가상의 XTS 모듈들을 추가하여 가상의 XTS 시스템을 구성합니다.   
I/O - Devices의 Context Menu에서 Add New Item...을 선택하고 대화상자에서 EtherCAT - EtherCAT Master를 선택합니다.   
<img src="https://user-images.githubusercontent.com/79190459/108970158-3b45d800-76c5-11eb-81db-c5fab7beb74e.png" width="40%">   
<img src="https://user-images.githubusercontent.com/79190459/108970178-3bde6e80-76c5-11eb-956b-0d7d3c062453.png" width="40%">

새로 추가된 EtherCAT Master에 새 모듈을 추가합니다.   
XTS - AT2001-0250 Motor module with feed 250mm, 48V를 선택하여 추가합니다.   
이 모듈은 전원선이 연결되며 일반적으로 무버들의 원점으로 사용됩니다.   
<img src="https://user-images.githubusercontent.com/79190459/108973227-75b07480-76c7-11eb-93f5-312ceb4d6682.png" width="40%">

AT2001-0250을 선택하고 AT2001-5250 Sensor line with feed를 추가합니다.   

이번 예제에서는 총 길이 3000mm의 트랙을 구성할 것입니다. 전체 트랙은 직선 모듈 8개와 90° clothoid 모듈 4개로 구성됩니다.   
<img src="https://user-images.githubusercontent.com/79190459/108975196-95489c80-76c9-11eb-92df-fa69175996fe.png" width="40%">

직선 모듈 3개를 더 추가해야 합니다. EtherCAT Master에 AT2000-0250 모듈 3개를 추가합니다. 각 모듈에는 AT2000-5250 Sensor Line 모듈을 추가합니다.   
곡선 모듈은 AT2050-0500 Motor module, 90° clothoid, EtherCAT In, 48V를 먼저 추가합니다. 그 다음 AT2050-0501 모듈을 추가합니다.   
마찬가지로 두 모듈에 적절한 Sensor Line 모듈을 추가합니다.

트랙의 절반을 완성했습니다. I/O - Devices에 EtherCAT Master를 하나 더 추가하고 같은 과정을 반복합니다.   
마지막으로 두 EtherCAT Master는 실제로 존재하는 장치가 아니므로 Context Menu에서 Disable을 선택합니다.

## XTS IO Driver 추가
