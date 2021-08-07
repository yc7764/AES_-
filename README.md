# AES(Advanced Encryption Standard)_128
AES는 암호 알고리즘으로 암호화와 복호화에 동일한 키를 사용하는 대칭키 알고리즘이다. 128bit의 평문을 입력으로 받아 암호화 과정을 통해 128bit의 암호문을 출력하거나 128bit의 암호문을 복호화 과정을 통해서 128bit의 평문을 출력한다. 각 라운드에 대응되는 키는 128, 192, 256bit가 있으며, 이 중에서 키의 길이가 128bit인 AES-128을 구현하였다.

## 시스템 구성도
프로그램 실행 시 KeyExpansion() 함수를 이용하여 128비트의 키로 44 word(1 word는 32bit)의 각 라운드 키를 생성하고 암호화를 시작한다. 먼저 AddRoundKey 함수를 호출하고, 총 10 라운드가 진행한다. 각 라운드는 순서대로 SubBytes, ShiftRows, MixColumns, AddRoundKey로 진행되며 마지막 라운드에서는 MixColumns를 제외한다.
  1. KeyExpansion  
     - 각 라운드에 사용하는 라운드 키를 생성하는 함수로 하나의 128비트 암호 키로부터 11개의 128비트 라운드 키를 생성하였다. 워드가 4개의 바이트로 이루어진다고 할 때, 키 확장 루틴은 워드 단위로        라운드키를 생성한다. 키 확장 과정에서 라운드의 수를 N이라고 할 때, 4x(N+1)개의 워드를 생성하며 총 10 라운드를 진행하기 위해서 총 44개의 워드를 생성하고 각 라운드 키는 네 개의 워드로 이        루어진다. 각 라운드에 사용되는 워드는 아래의 표와 같다.  

        | Round     | Words                 |
        |-----------|-----------------------|
        | Pre-round | W<sub>0</sub>  W<sub>1</sub>  W<sub>2</sub>  W<sub>3</sub> |
        | 1         | W<sub>4</sub>  W<sub>5</sub>  W<sub>6</sub>  W<sub>7</sub> |
        | 2         | W<sub>8</sub>  W<sub>9</sub>  W<sub>10</sub>  W<sub>11</sub> |
        | 3         | W<sub>12</sub>  W<sub>13</sub>  W<sub>14</sub>  W<sub>15</sub> |
        | N         | W<sub>4N</sub>  W<sub>4N+1</sub>  W<sub>4N+2</sub>  W<sub>4N+3</sub>|
     - 처음 네 개의 워드 W<sub>0</sub>, W<sub>1</sub>, W<sub>2</sub>, W<sub>3</sub>는 암호 키로부터 만들어지며 모든 과정은 아래의 그림과 같다. 암호키의 처음 4바이트는 W<sub>0</sub>가 되고        그 다음 4바이트는 W<sub>1</sub>이 되는 과정을 통해 W<sub>3</sub>까지 정의를 한다. 나머지 워드들은 다음의 과정을 통해 계산한다.
       - i mod 4 ≠ 0일 경우, W<sub>i</sub> = W<sub>i-1</sub> ⊕ W<sub>i-4</sub>
       - i mod 4 = 0일 경우, W<sub>i</sub> = t ⊕ W<sub>i-4</sub>, 이떄 t는 W<sub>i-1</sub>를 이용하여 구한다. W<sub>i</sub>를 왼쪽으로 한바이트씩 이동시키고 각 바이트를 s_box를 이용하여          대치시킨다. 그리고 대치시킨 값과 라운드 상수 rcon을 배타적 논리합한 결과가 t이다.  
         ![image](https://user-images.githubusercontent.com/59434021/128598103-ec3c42a3-c817-470a-8651-5f1076817103.png)
  2. AddRoundKey
    - 
  3. SubBytes
    - 
  4. ShiftRows
    - 
  5. MixColumns

## 실행 결과
