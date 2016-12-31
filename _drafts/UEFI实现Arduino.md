初步计划:
1.实现超级终端功能

void setup() {
  // put your setup code here, to run once:
  Serial.begin(9600);
}

void loop() {
  // put your main code here, to run repeatedly:
  if (Serial.available()) {
    char ch = Serial.read();
    if((ch >= '0') && (ch <= '9'))
      Serial.println(" :-> The Numbers!!"); 
    else
      Serial.println(" :-> Others!!");
    
    }
}
已完成minicom配置使用,sreen, cu也有同样的功能

2.加载Application代码,实现Shell功能

3.摆脱Arduino IDE
实现代码上传,Emacs 下编写编译
/usr/share/doc/avr-libc/avr-libc-user-manual/ avr-gcc参考文档位置

avrdude -c arduino -p atmega328p -P /dev/ttyACM0 -F -u -U flash:w:blink.hex

/usr/lib/avr/include/avr/
实现UEFI的入口点, 模拟UEFI启动环境.

USART 代码实现主要以Datasheet的样例,其中USARTn要代替为实际的USART0寄存器的值,
FOSC的大小为16MHZ 从电路图的外部驱动时钟可以看出,也可以参看Arduino的技术规格,
时钟愿的配置由CKSEL[3:0] 控制,它是Fuse bit的一部分,通过编程器arvdude 可以设定.
4.实现bootloader



明确哪些是系统架构定义的,哪些是soc厂商定义的功能
