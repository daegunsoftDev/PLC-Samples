# PLC-Practice-NetworkVariablesSender
 EAP NetworkVariables Sender

## 사용법
1. Solution Explorer - I/O - Devices에 EtherCAT - EtherCAT Automation Protocol (Network Variables) 추가.
<img src="https://user-images.githubusercontent.com/79190459/108932473-c9ea3300-768c-11eb-8f0a-93fb0ccdc9f4.gif" width="50%">
2. 추가된 장치 선택 후 Adapter 탭에서 Search... 버튼 클릭 후 적절한 항목 선택.
<img src="https://user-images.githubusercontent.com/79190459/108932487-ce165080-768c-11eb-8831-7c2f3208db72.gif" width="50%">
3. 추가된 장치에 Network Variable Publisher 추가.
<img src="https://user-images.githubusercontent.com/79190459/108932493-d078aa80-768c-11eb-8d3a-059827b2c842.gif" width="50%">
4. 필요한 경우 추가된 Box 선택 후 Publish 탭 - Sending Options 수정.
    * Broadcast는 불특정 다수에게 송신
    * Multicast는 같은 그룹에게 송신
    * Unicast는 특정 장치에게 송신
5. Box에 송신할 변수를 추가.
<img src="https://user-images.githubusercontent.com/79190459/108932501-d2426e00-768c-11eb-9b2a-6cf723fccdf7.gif" width="50%">
6. 추가된 항목의 Output - VarData를 PLC 변수와 Link.

## 주의사항
* Target Device가 Local인 경우 정상적으로 동작하지 않음.

## 참조
https://infosys.beckhoff.com/english.php?content=../content/1033/eap/index.html

https://www.plccoder.com/communicating-between-beckhoff-controllers-via-eap/
