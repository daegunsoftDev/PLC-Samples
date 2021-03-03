# PLC-Practice-NetworkVariablesSender
 EAP NetworkVariables Sender

## 사용법
1. Solution Explorer - I/O - Devices에 EtherCAT - EtherCAT Automation Protocol (Network Variables) 추가.   
<img src="Guide/1. EAP 추가.gif" width="75%">   
2. 추가된 장치 선택 후 Adapter 탭에서 Search... 버튼 클릭 후 적절한 항목 선택.   
<img src="Guide/2. Network Adapter 설정.gif" width="75%">   
3. 추가된 장치에 Network Variable Publisher 추가.   
<img src="Guide/3. Network Variable Publisher 추가.gif" width="75%">   
4. 필요한 경우 추가된 Box 선택 후 Publish 탭 - Sending Options 수정.   

  - Broadcast는 불특정 다수에게 송신
  - Multicast는 같은 그룹에게 송신
  - Unicast는 특정 장치에게 송신
5. Box에 송신할 변수를 추가.
<img src="Guide/4. Box에 변수 추가.gif" width="75%">
6. 추가된 항목의 Output - VarData를 PLC 변수와 Link.

## 주의사항
* Target Device가 Local인 경우 정상적으로 동작하지 않음.

## 참조
https://infosys.beckhoff.com/english.php?content=../content/1033/eap/index.html

https://www.plccoder.com/communicating-between-beckhoff-controllers-via-eap/
