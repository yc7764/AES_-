# AES(Advanced Encryption Standard)_128
AES는 암호 알고리즘으로 암호화와 복호화에 동일한 키를 사용하는 대칭키 알고리즘이다. 128bit의 평문을 입력으로 받아 암호화 과정을 통해 128bit의 암호문을 출력하거나 128bit의 암호문을 복호화 과정을 통해서 128bit의 평문을 출력한다. 각 라운드에 대응되는 키는 128, 192, 256bit가 있으며, 이 중에서 키의 길이가 128bit인 AES-128을 구현하였다.

## 시스템 구성도
프로그램 실행 시 KeyExpansion() 함수를 이용하여 128비트의 키로 44 word(1 word는 32bit)의 각 라운드 키를 생성하고 암호화를 시작한다. 먼저 AddRoundKey 함수를 호출하고, 총 10 라운드가 진행한다. 각 라운드는 순서대로 SubBytes, ShiftRows, MixColumns, AddRoundKey로 진행되며 마지막 라운드에서는 MixColumns를 제외한다.
  1. KeyExpansion
    - 
  2. AddRoundKey
    - 
  3. SubBytes
    - 
  4. ShiftRows
    - 
  5. MixColumns

##실행 결과
