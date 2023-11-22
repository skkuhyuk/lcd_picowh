C를 사용한 이유
-
1. PicoWh를 16bit RGB를 사용시 필요한 buffer는 320*240*2(byte)로 153600byte가 필요한데, 이는 PicoWh의 RAM을 초과하여 구동이 안됨

1-1. Width나 height를 줄이면 화면이 깨져 글씨를 알아보지 못 하거나 글씨 크기가 과도하게 작아져서 보이지 않음.

1-2. ST7789VW는 8bit, 16bit, 18bit의 RGB를 사용할 수 있는데 8bit 사용시 기존 code와 충돌 발생

2. LCD.text의(micropython) font크기를 변경 불가능. 다른 사람이 찍어놓은 도트 font를 다운받아서 사용해야 하는데, driver 충돌 발생 

3. PicoWH/ ST7789VW/ micropython을 사용한 모든 github/youtube를 따라가 봤지만 import 에러 발생. 

    
중요)  https://www.waveshare.com/wiki/Pico-LCD-2 를 따라가면 안됨. C에서도 cmake가 인식이 안됨. 
-
SDK_PATH를 설정하는 데 있어서서 export를 하기 직전에 pico-sdk를 복제하여 현재 folder의 위치에 넣어야함. 이렇게 하지 않으면 설치되어 있어도 파일 인식 못함. 

**run을 하면 compile error가 뜨기에, /build 에 들어가 cmd에서 make로 uf2를 생성해야한다. **

현재 구현 정보
-

1. init이 끝나면, loop로 Wifi 연결 유무 값(int or bool) 확인
2. Wifi의 연결 유무 값(int or bool)에따라 다른 출력값 도출

   -temp로 ON/OFF의 값 뜨게 함. OFF일 경우 lcd를 끄게 할 예정
   
3-1. JPG가 unsigned int(RGB factor) array로 나타내졌을 경우, LCD에 뜨게 구현-> 이는 White or Black으로 JPG만들어서 RGB data가 0xFFFF(white)아닌 bit만 검은색으로 변환하도록 하면 될 것 같고,
LCD화면 하얀색으로 clear한 후에 숫자 있는 부분만 bit를 찍어도 될것 같다. 아니면 숫자를 더 크게 bitmap으로 만들어서 display하면 될 듯 싶다. 

3-2 string을 LCD에 나타냄. font최대 크기 24, 더 큰 font를 더 찾아볼 예정 



->Pico-LCD-2.py로 해결.
