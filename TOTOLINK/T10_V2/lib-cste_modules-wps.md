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




    
    

  
