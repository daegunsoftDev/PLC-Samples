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


## XTS IO Driver 추가
