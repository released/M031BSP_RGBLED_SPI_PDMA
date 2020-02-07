# M031BSP_RGBLED_SPI_PDMA
 M031BSP_RGBLED_SPI_PDMA

update @ 2020/02/07

1. Use SPI MOSI (PA0) with PDMA , to send WS2812 data 

- T1H 1 HIGH 580ns~1μs		target : 0.750 us

- T1L 1 LOW 220ns~420ns		target : 0.375 us

1111 1100 (0xFC)	==> if SPI = 7M

- T0H 0 HIGH 220ns~380ns		target : 0.375 us

- T0L 0 LOW 580ns~1μs			target : 0.750 us

1100 0000 (0xC0)	==> if SPI = 7M	

- RES : more than 280μs

2. upload picture (bit 1 , bit 0) for reference

- bit 1 : ideal high level 0.375x2 us , low level 0.375x1 us

- bit 0 : ideal high level 0.375x1 us , low level 0.375x2 us

![image](https://github.com/released/M031BSP_RGBLED_SPI_PDMA/blob/master/M031_RGB_1LED_0xFF_0x00_0x00.jpg)

![image](https://github.com/released/M031BSP_RGBLED_SPI_PDMA/blob/master/scope_bit_High_Low.jpg)

3. Upon picture , 2nd channel is PA2 (SPI CLK) 

4. SPI with PDMA concept

- convert RGB data into SPI data (0xFC , 0xC0) , prepare SPI data before each PDMA transfer

- 1 LED = 3 pixel R , G , B 

- each pixel (ex : R) , need 8 bytes SPI data transfer

- 1 LED = 3 * 8 = 24 bytes for SPI data

5. For example : 

if RGB data G = 0x00 , R = 0xFF , B = 0x00 , SPI data will be 24 bytes , as below

0xC0 ,0xC0 ,0xC0 ,0xC0 ,0xC0 ,0xC0 ,0xC0 ,0xC0 ,

0xFC ,0xFC ,0xFC ,0xFC ,0xFC ,0xFC ,0xFC ,0xFC ,

0xC0 ,0xC0 ,0xC0 ,0xC0 ,0xC0 ,0xC0 ,0xC0 ,0xC0 ,

##Scenario : 

1. Default LED pattern will show index state_AllColors in StateMachine function

2. By pressing keyboard from terminal (through UART0 , key from 1 ~ 9 , a ~ d ) to display each LED pattern

