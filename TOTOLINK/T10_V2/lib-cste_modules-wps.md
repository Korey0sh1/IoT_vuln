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
![](https://github.com/Korey0sh1/IoT_vuln/blob/main/TOTOLINK/T10_V2/0.jpg)



    
    

  
