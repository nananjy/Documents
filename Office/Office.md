# Office使用密钥校验

## 验证解密

- 管理员账号登录CMD
	菜单 ——》 CMD右键 ——》 以管理员身份运行

- 根据系统位数和安装office位数执行 cscript "...OSPP.VBS" /dstatus
```
Microsoft Windows [版本 10.0.18362.1139]
(c) 2019 Microsoft Corporation。保留所有权利。

C:\WINDOWS\system32>cscript "C:\Program Files (x86)\Microsoft Office\Office16\OSPP.VBS" /dstatus
Microsoft (R) Windows Script Host Version 5.812
版权所有(C) Microsoft Corporation。保留所有权利。

---Processing--------------------------
---------------------------------------
PRODUCT ID: 00341-37441-80031-AA583
SKU ID: 2dfe2075-2d04-4e43-816a-eb60bbb77574
LICENSE NAME: Office 16, Office16VisioProR_Retail edition
LICENSE DESCRIPTION: Office 16, RETAIL channel
BETA EXPIRATION: 1601/1/1
LICENSE STATUS:  ---NOTIFICATIONS---
ERROR CODE: 0xC004F009
ERROR DESCRIPTION: The Software Licensing Service reported that the grace period expired.
Last 5 characters of installed product key: Y984V
---------------------------------------
PRODUCT ID: 00346-80000-00010-AA673
SKU ID: 5821ec16-77a9-4404-99c8-2756dc6d4c3c
LICENSE NAME: Office 16, Office16VisioProR_Grace edition
LICENSE DESCRIPTION: Office 16, RETAIL(Grace) channel
BETA EXPIRATION: 1601/1/1
LICENSE STATUS:  ---NOTIFICATIONS---
ERROR CODE: 0xC004F009
ERROR DESCRIPTION: The Software Licensing Service reported that the grace period expired.
Last 5 characters of installed product key: F6YYT
---------------------------------------
PRODUCT ID: 00341-50000-00000-AA019
SKU ID: 6bf301c1-b94a-43e9-ba31-d494598c47fb
LICENSE NAME: Office 16, Office16VisioProVL_KMS_Client edition
LICENSE DESCRIPTION: Office 16, VOLUME_KMSCLIENT channel
BETA EXPIRATION: 1601/1/1
LICENSE STATUS:  ---NOTIFICATIONS---
ERROR CODE: 0xC004F00F
ERROR DESCRIPTION: The Software Licensing Service reported that the hardware ID binding is beyond the level of tolerance.
Last 5 characters of installed product key: RJRJK
Activation Type Configuration: ALL
        DNS auto-discovery: KMS name not available
        Activation Interval: 120 minutes
        Renewal Interval: 10080 minutes
        KMS host caching: Enabled
---------------------------------------
PRODUCT ID: 00200-70000-00000-AA752
SKU ID: d7279dd0-e175-49fe-a623-8fc2fc00afc4
LICENSE NAME: Office 16, Office16O365HomePremR_Grace edition
LICENSE DESCRIPTION: Office 16, RETAIL(Grace) channel
BETA EXPIRATION: 1601/1/1
LICENSE STATUS:  ---NOTIFICATIONS---
ERROR CODE: 0xC004F009
ERROR DESCRIPTION: The Software Licensing Service reported that the grace period expired.
Last 5 characters of installed product key: KHGM9
---------------------------------------
---------------------------------------
---Exiting-----------------------------
```
- 执行 slmgr /xpr xxxxxxxxxxxxxxxxxxxxx(sku id) ，回车
```
slmgr /xpr 2dfe2075-2d04-4e43-816a-eb60bbb77574
> Office 16, Office16VisioProR_Retail edition:Windows处于通知模式
slmgr /xpr 5821ec16-77a9-4404-99c8-2756dc6d4c3c
> Office 16, Office16VisioProR_Grace edition:Windows处于通知模式
slmgr /xpr 6bf301c1-b94a-43e9-ba31-d494598c47fb
> Office 16, Office16VisioProVL_KMS_Client edition:Windows处于通知模式
slmgr /xpr d7279dd0-e175-49fe-a623-8fc2fc00afc4
> Office 16, Office16O365HomePremR_Grace edition:Windows处于通知模式
- 查看windows是否解密 slmgr.vbs -xpr
> Windows(R), Professional edition:计算机已永久激活。
```



