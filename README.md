# AES(Advanced Encryption Standard)_128
AES는 암호 알고리즘으로 암호화와 복호화에 동일한 키를 사용하는 대칭키 알고리즘이다. 128bit의 평문을 입력으로 받아 암호화 과정을 통해 128bit의 암호문을 출력하거나 128bit의 암호문을 복호화 과정을 통해서 128bit의 평문을 출력한다. 각 라운드에 대응되는 키는 128, 192, 256bit가 있으며, 이 중에서 키의 길이가 128bit인 AES-128을 구현하였다.

## 시스템 구성도
프로그램 실행 시 KeyExpansion() 함수를 이용하여 128bit의 키로 44 word(1 word는 32bit)의 각 라운드 키를 생성하고 암호화를 시작한다. 먼저 AddRoundKey 함수를 호출하고, 총 10 라운드가 진행한다. 각 라운드는 순서대로 SubBytes, ShiftRows, MixColumns, AddRoundKey로 진행되며 마지막 라운드에서는 MixColumns를 제외한다.
  1. KeyExpansion  
     - 각 라운드에 사용하는 라운드 키를 생성하는 함수로 하나의 128비트 암호 키로부터 11개의 128비트 라운드 키를 생성하였다. 워드가 4개의 바이트로 이루어진다고 할 때, 키 확장 루틴은 워드 단위로 라운드키를 생성한다. 키 확장 과정에서 라운드의 수를 N이라고 할 때, 4x(N+1)개의 워드를 생성하며 총 10 라운드를 진행하기 위해서 총 44개의 워드를 생성하고 각 라운드 키는 네 개의 워드로 이루어진다. 각 라운드에 사용되는 워드는 아래의 표와 같다.  

        | Round     | Words                 |
        |-----------|-----------------------|
        | Pre-round | W<sub>0</sub>  W<sub>1</sub>  W<sub>2</sub>  W<sub>3</sub> |
        | 1         | W<sub>4</sub>  W<sub>5</sub>  W<sub>6</sub>  W<sub>7</sub> |
        | 2         | W<sub>8</sub>  W<sub>9</sub>  W<sub>10</sub>  W<sub>11</sub> |
        | 3         | W<sub>12</sub>  W<sub>13</sub>  W<sub>14</sub>  W<sub>15</sub> |
        | N         | W<sub>4N</sub>  W<sub>4N+1</sub>  W<sub>4N+2</sub>  W<sub>4N+3</sub>|
     - 처음 네 개의 워드 W<sub>0</sub>, W<sub>1</sub>, W<sub>2</sub>, W<sub>3</sub>는 암호 키로부터 만들어지며 모든 과정은 아래의 그림과 같다. 암호키의 처음 4바이트는 W<sub>0</sub>가          되고 그 다음 4바이트는 W<sub>1</sub>이 되는 과정을 통해 W<sub>3</sub>까지 정의를 한다. 나머지 워드들은 다음의 과정을 통해 계산한다.
       - i mod 4 ≠ 0일 경우, W<sub>i</sub> = W<sub>i-1</sub> ⊕ W<sub>i-4</sub>
       - i mod 4 = 0일 경우, W<sub>i</sub> = t ⊕ W<sub>i-4</sub>, 이떄 t는 W<sub>i-1</sub>를 이용하여 구한다. W<sub>i</sub>를 왼쪽으로 한바이트씩 이동시키고 각 바이트를 s_box를 이용하         여 대치시킨다. 그리고 대치시킨 값과 라운드 상수 rcon을 배타적 논리합한 결과가 t이다.  
         ![image](https://user-images.githubusercontent.com/59434021/128598103-ec3c42a3-c817-470a-8651-5f1076817103.png)  
  2. AddRoundKey
     - KeyExpansion 함수에 의해 생성된 각 라운드 키를 이용하여 해당 라운드 state 행렬의 각 열에 xor 연산을 한다.  
       ![image](https://user-images.githubusercontent.com/59434021/129000253-f6cfad02-f6f4-4e11-9030-96e8dcd49528.png)  
  3. SubBytes
     - 하나의 byte를 대치하기 위해 각 byte를 4bit씩 2개의 16진수로 계산하여 왼쪽 4bit를 s_box의 행으로 오른쪽 4bit를 열로 표를 읽어온다. s_box는 아래의 표와 같다.  
     ![image](https://user-images.githubusercontent.com/59434021/129005468-837df6a2-ecbb-40f4-9723-8085670e6c2c.png)  
     - s_box  


          |   | 0  | 1  | 2  | 3  | 4  | 5  | 6  | 7  | 8  | 9  | A  | B  | C  | D  | E  | F  |
          |---|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
          | 0 | 63 | 7C | 77 | 7B | F2 | 6B | 6F | C5 | 30 | 01 | 67 | 2B | FE | D7 | AB | 76 |
          | 1 | CA | 82 | C9 | 7D | FA | 59 | 47 | F0 | AD | D4 | A2 | AF | 9C | A4 | 72 | C0 |
          | 2 | B7 | FD | 93 | 26 | 36 | 3F | F7 | CC | 34 | A5 | E5 | F1 | 71 | D8 | 31 | 15 |
          | 3 | 04 | C7 | 23 | C3 | 18 | 96 | 05 | 9A | 07 | 12 | 80 | E2 | EB | 27 | B2 | 75 |
          | 4 | 09 | 83 | 2C | 1A | 1B | 6E | 5A | A0 | 52 | 3B | D6 | B3 | 29 | E3 | 2F | 84 |
          | 5 | 53 | D1 | 00 | ED | 20 | FC | B1 | 5B | 6A | CB | BE | 39 | 4A | 4C | 58 | CF |
          | 6 | D0 | EF | AA | FB | 43 | 4D | 33 | 85 | 45 | F9 | 02 | 7F | 50 | 3C | 9F | A8 |
          | 7 | 51 | A3 | 40 | 8F | 92 | 9D | 38 | F5 | BC | B6 | DA | 21 | 10 | FF | F3 | D2 |
          | 8 | CD | 0C | 13 | EC | 5F | 97 | 44 | 17 | C4 | A7 | 7E | 3D | 64 | 5D | 19 | 73 |
          | 9 | 60 | 81 | 4F | DC | 22 | 2A | 90 | 88 | 46 | EE | B8 | 14 | DE | 5E | 0B | DB |
          | A | E0 | 32 | 3A | 0A | 49 | 06 | 24 | 5C | C2 | D3 | AC | 62 | 91 | 95 | E4 | 79 |
          | B | E7 | C8 | 37 | 6D | 8D | D5 | 4E | A9 | 6C | 56 | F4 | EA | 65 | 7A | AE | 08 |
          | C | BA | 78 | 25 | 2E | 1C | A6 | B4 | C6 | E8 | DD | 74 | 1F | 4B | BD | 8B | 8A |
          | D | 70 | 3E | B5 | 66 | 48 | 03 | F6 | 0E | 61 | 35 | 57 | B9 | 86 | C1 | 1D | 9E |
          | E | E1 | F8 | 98 | 11 | 69 | D9 | 8E | 94 | 9B | 1E | 87 | E9 | CE | 55 | 28 | DF |
          | F | 8C | A1 | 89 | 0D | BF | E6 | 42 | 68 | 41 | 99 | 2D | 0F | B0 | 54 | BB | 16 |
  4. ShiftRows
     - 행 단위 연산을 수행하여 각 state 행렬의 행을 바이트 이동시킨다. 이때 이동하는 수는 행렬의 행 번호(0~3)에 의존하며, 0번쨰 행에서는 이동하지 않고, 마지막 행에서는 3byte만큼 순환 이동한다.
     ![image](https://user-images.githubusercontent.com/59434021/129010266-abd87335-1b8b-4786-bc5b-6e78fe496ed6.png)  
  5. MixColumns
     - 열 단위 연산을 수행하여 각 state 행렬의 열을 새로운 열로 변환한다. state 행렬에 상수 정방 행렬을 곱하는 연산이다. 이때 byte들의 곱셈은 x<sup>8</sup>+x<sup>4</sup>+x<sup>3</sup>+x+1을 가지고 GF(2<sup>8</sup>)에서 행해지고 덧셈은 8bit인 word들의 xor연산과 동일하다.
     ![image](https://user-images.githubusercontent.com/59434021/129012520-e39742ae-0a1c-411b-b0ea-50beacf1e83f.png)
