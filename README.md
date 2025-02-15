# IP APB-UART
APB-UART is a bridge that converts between the protocols APB and UART. APB interface of ip is design by APB 4 specifications.
That brigde have total five interrupt signals.
## Pin diagram of IP
Pin diagram of IP is showed below:  
![pindiagram](https://github.com/hoanvipro/APB_UART_Final/assets/130170857/6ceb3305-9aa6-4562-ba94-6ec935376bed)  
PCLK, PRESET is signal from system. PSEL, PWRITE, PENABLE, PADDR, PWDATA, PSTRB, PPROT, PRDATA, PREADY, PSLVERR are connected with bus APB. TXD and RXD are connected with UART peripheral.  
Detail of six interrupt signals:
|Name|Description|
|:-|:-|
|I_RXOV|Overflow interrupt|
|I_TXTHR|Threshold of Transmitter interrupt|
|I_RXTHR|Threshold of Receiver interrupt|
|I_FE|Frame error of UART data interrupt|
|I_PE|Parity error of UART data interrupt|
|I_TOTAL|Total interrupt, high when any other interrupt signals high|
## Registers of IP
There are total 4 registers in IP:
|Name Register|Type|Length|Address|Description|
|:-|:-|:-|:-|:-|
|REG_DATA|Read/Write|8 bits|0x00000000|Register temporarily stores data|
|REG_BCLK|Read/Write|11 bits|0x00000004|Register stores BCLK value|
|REG_EN|Read/Write|8 bits|0x00000008|Register stores enable signals|
|REG_THR|Read/Write|4 bits|0x0000000C|Register stores threshold for transmitter and receiver|
## Setting baudrate and enable signals
### Setting baudrate
Baudrate is setting with equation: BCLK_VAL = (ClockFrequency)/(Baudrate*16)
** BCLK_VAL: value that user need setting for IP.
** ClockFrequency: System frequency.
** Baudrate: Value that user want to setting.  
Example: If user want to set baudrate with value is 9600 and system frequency is 100 MHz. First, user need to calculate BCLK_VAL = (ClockFrequency)/(Baudrate*16) = (100M)/(9600*16) = 651.  
Then, user will set BCLK_VAL into BCLK_REG.  
### Setting enable signals
There are 8 enables signal can be set:
|Name Register|Bit position in REG_EN[7:0]|Description|
|:-|:-|:-|
|TXTHR_EN|REG_EN[0]|Enable threshold interrupt signal in Transmitter|
|RXTHR_EN|REG_EN[1]|Enable threshold interrupt signal in Receiver|
|RXOV_EN|REG_EN[2]|Enable overflow interrupt signal in Receiver|
|PE_EN|REG_EN[3]|Enable parity error interrupt signal in Receiver|
|FE_EN|REG_EN[4]|Enable frame error interrupt signal in Receiver|
|IP_EN|REG_EN[5]|Enable ip active|
|PARITY_EN|REG_EN[6]|Enable trans and recive data with parity|
|PARITY_TYPE|REG_EN[7]|Type of parity, 0 is odd parity and 1 is even parity|
## Testing IP
IP works properly with all 17 test cases:
|No.|Name|
|:-|:-|
|1|Trans data from APB to UART with no parity|
|2|Trans data from APB to UART with even parity|
|3|Trans data from APB to UART with odd parity|
|4|Trans 2 data from APB to UART with no parity|
|5|Receive data from UART to APB with no parity|
|6|Receive data from UART to APB with even parity|
|7|Receive data from UART to APB with odd parity|
|8|Receive 2 data from UART to APB with no parity|
|9|Receive data from UART to APB with no parity and other baudrate|
|10|Trans and receive data as same time|
|11|Receive fake start bit from UART|
|12|Active I_TXTHR signal|
|13|Active I_RXTHR signal|
|14|Active I_RXOV signal|
|15|Active I_PE signal|
|16|Active I_FE signal|
|17|Active PSLVERR signal|  
## Some images of result.
The enviroment testing is built with 100MHz for system frequency and 460800 for default baudrate (BCLK_VAL = 14).  
There are some image of result:
### Trans data from APB to UART with no parity:
Setting enable signal, baudrate and data in APB interface:
![settingtransnoparity](https://github.com/hoanvipro/APB_UART_Final/assets/130170857/7df3caac-a3b2-487d-af74-a713756f5e34)
Result in TXD:
![txdtransnoparity](https://github.com/hoanvipro/APB_UART_Final/assets/130170857/baf93d8a-45bd-495e-a7da-69dd8035cbe3)
### Receive data from APB to UART with no parity:
Setting enable signal, baudrate:
![settingreceivenoparity](https://github.com/hoanvipro/APB_UART_Final/assets/130170857/a31db284-7526-42ae-ae23-5f52a9d3a803)
Data in RXD:
![rxdreceivenoparity](https://github.com/hoanvipro/APB_UART_Final/assets/130170857/65c59c7c-f45b-4ced-91fa-fdc4c436a4c7)
Read data in APB:
![readapbnoparity](https://github.com/hoanvipro/APB_UART_Final/assets/130170857/79aed659-c9f0-41f6-a278-ed71d6a3c52f)
## Eviroment testing using UVM
This simulation environment is still under construction and will be completed in the future.
## Reference
[1] 	ARM Limited, "AMBA Specification", ARM IHI 0024C, 1999.   
[2] 	S, Kavyashree, "Design and Implementation of UART using Verilog", 12 December, 2015.   
[3] 	Bhargav Tarpara, Heli ShahP and Chinmay ModiP, "Design & Implementation of Advance Peripheral Bus Protocol", June 2015.   
[4] 	ARM Limited, AMBA APB Protocol, 2003 - 2010.   
[5] 	Texas Instruments, KeyStone Architecture Universal Asynchronous Receiver/Transmitter (UART) User Guide, Nov. 2010.   
[6] 	Institute of Electrical and Electronics Engineers, Inc, "IEEE Standard for Verilog Hardware Description Language", 7 April 2006.   
[7] 	Mary Grace Legaspi and Eric Peňa, "UART: A Hardware Communication Protocol Understanding Universal Asynchronous Receiver/Transmitter", December 2020.   
[8] 	Neeta Choubey and H.R.Singh, "IMPLEMENTATION OF UART WITH STATUS REGISTER AND APB", July-August 2015.   
[9] 	Anushka Dwivedi and Dr. Anand Jatti, "DESIGN AND IMPLEMENTATION OF AMBA APB PROTOCOL", July-2022.   
[10]  Nguyễn Quân, "[UVM] Bài 1 - Tổng quan về UVM và mô tả lõi DUT UART-APB", June 2019.  
