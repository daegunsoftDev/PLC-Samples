# PLC-Practice-NetworkVariablesReceiver
 EAP NetworkVariables Receiver

## 사용법
1. Solution Explorer - I/O - Devices에 EtherCAT - EtherCAT Automation Protocol (Network Variables) 추가.
2. 추가된 장치 선택 후 Adapter 탭에서 Search... 버튼 클릭 후 적절한 항목 선택.
3. 추가된 장치에 Network Variable Subscriber 추가.
4. Box 우클릭 - Add new item... - Browse for Computer... - Publisher 선택.
5. 창 하단 변수 목록에서 원하는 항목 선택하여 추가.
6. 필요한 경우 추가된 Box 선택 후 Subscribe 탭의 옵션 수정.
    * Multicast 사용시 그룹 ID가 서로 같아야 함.
7. 추가된 항목의 Input - VarData를 PLC 변수와 Link.

## 주의사항
* Target Device가 Local인 경우 정상적으로 동작하지 않음.

## 참조
https://infosys.beckhoff.com/english.php?content=../content/1033/eap/index.html

https://www.plccoder.com/communicating-between-beckhoff-controllers-via-eap/
