<img width="1474" height="970" alt="image" src="https://github.com/user-attachments/assets/cc859010-eed1-46bd-b11e-f4f4930a0551" />
<img width="1456" height="971" alt="image" src="https://github.com/user-attachments/assets/b26b4b43-23a4-4485-b730-3c7def38dc6e" />
<img width="1479" height="973" alt="image" src="https://github.com/user-attachments/assets/604ad52b-dc73-4ce9-a641-61d984782471" />
<img width="1453" height="976" alt="image" src="https://github.com/user-attachments/assets/d1be62e4-1ed1-43ed-992b-b400daa186b9" />
<img width="1455" height="961" alt="image" src="https://github.com/user-attachments/assets/3d261b52-31cf-41e6-b669-1862b97d277a" />
<img width="1464" height="943" alt="image" src="https://github.com/user-attachments/assets/77972011-9b4f-42a5-b012-0d60e9bb805a" />
<img width="1454" height="896" alt="image" src="https://github.com/user-attachments/assets/4f6c4e1f-7f5b-4464-9fcb-2c24b2b421ec" />
<img width="1444" height="951" alt="image" src="https://github.com/user-attachments/assets/f1c19b35-2d02-456c-9c7d-117eff9370ea" />
<img width="1432" height="926" alt="image" src="https://github.com/user-attachments/assets/035a2df7-1188-48a3-9f30-c5ef300d6023" />
<img width="1455" height="933" alt="image" src="https://github.com/user-attachments/assets/8af98a02-af7f-466d-a0e1-7a1f738b82c5" />
<img width="1457" height="909" alt="image" src="https://github.com/user-attachments/assets/d6e48a58-b95a-484e-994f-71ef5b5de138" />
<img width="1440" height="925" alt="image" src="https://github.com/user-attachments/assets/ebbd2b08-c1eb-4209-8461-811fe881e121" />
<img width="1437" height="960" alt="image" src="https://github.com/user-attachments/assets/e4524718-cee6-428a-b2de-5e198189cae1" />
<img width="1734" height="928" alt="image" src="https://github.com/user-attachments/assets/f722c80d-0317-4808-9490-b7cc6fb21b76" />
<img width="1603" height="981" alt="image" src="https://github.com/user-attachments/assets/f3278e77-f442-40dc-bb18-225a17b19e58" />
<img width="1413" height="928" alt="image" src="https://github.com/user-attachments/assets/745c245b-37fc-4348-a067-1f2c0bda1616" />
<img width="1434" height="958" alt="image" src="https://github.com/user-attachments/assets/c2a52118-f991-4279-bad8-02c81993d3f8" />
<img width="1450" height="934" alt="image" src="https://github.com/user-attachments/assets/52e7375e-e580-4be3-8bad-a9f1335a61c6" />


ШПОРА С КОМАНДАМИ ЧИСТО ( СТАРАЯ, НОВАЯ НИЖЕ):

ISP

vim /etc/sysconfig/network – меняем hostname на ISP

vim /etc/hostname – меняем имя на ISP

reboot

Запускаем rtr-ы


ISP

vim /etc/net/ifaces/ens18/options – <img width="133" height="52" alt="image" src="https://github.com/user-attachments/assets/485f24a4-acbb-4b16-afa3-3fe23fa02904" />

vim /etc/net/ifaces/ens18/resolv.conf <img width="158" height="31" alt="image" src="https://github.com/user-attachments/assets/67dbf4ea-2153-4196-9e79-ec66c634d825" />

mkdir /etc/net/ifaces/ens19 

vim /etc/net/ifaces/ens19/ipv4address – <img width="179" height="34" alt="image" src="https://github.com/user-attachments/assets/7fb85351-98b5-4c17-ab45-794851646e8e" />

vim /etc/net/ifaces/ens19/options - <img width="140" height="47" alt="image" src="https://github.com/user-attachments/assets/ab794599-9f77-41b0-aaf1-9205779b1635" />

mkdir /etc/net/ifaces/ens20 

vim /etc/net/ifaces/ens20/ipv4address - <img width="166" height="34" alt="image" src="https://github.com/user-attachments/assets/db353d44-4e53-4be5-b940-60709de968d0" />

vim /etc/net/ifaces/ens20/options - <img width="140" height="47" alt="image" src="https://github.com/user-attachments/assets/01336682-0206-410c-a2d4-a35a00eeb8a8" />

vim etc/net/sysctl.conf - <img width="316" height="28" alt="image" src="https://github.com/user-attachments/assets/cc75eee2-0c5a-4c52-963a-c8e25be82ad7" />

systemct restart network (service network restart)

apt-get update

apt-get install iptables –y

iptables –t nat –A POSTROUTING –o ens18 –j MASQUERADE

iptables-save >> /etc/sysconfig /iptables

systemctl enable --now iptables

systemctl restart network

iptables –t nat –L –n –v – проверка наличия правил

reboot


_________________________________
Переходим к настройке Hq-rtr 

hostname hq-rtr.au-team.irpo

do show port brief (первый порт идет в сторону isp)

int ISP

ip address 172.16.4.2/28

ex

port te0

servise-instance te0/ISP

encapsulation untagged

connect ip interface ISP

int vl100

ip address 172.17.250.65/26

int vl200

ip address 172.17.250.161/28

int vl999

ip address 172.17.250.177/29

port te1 

service-instance te1/vl100

encapsulation dot1q vl100

rewrite pop 1

connect ip int vl100

ex

service-instance te1/vl200

encapsulation dot1q vl200

rewrite pop 1

connect ip int vl200

ex

service-instance te1/vl999

encapsulation dot1q vl999

rewrite pop 1

connect ip int vl999

ex


ex

ip route 0.0.0.0/0 172.16.4.1

ip name-server 77.88.8.8

show ip int brief – должно быть так <img width="504" height="103" alt="image" src="https://github.com/user-attachments/assets/77ef0a6e-b347-441a-8773-77ec71de976e" />


так же стоит проверить ping isp-rtr

wr memory
____________________________________
Переходим к настройке Br-rtr 

hostname br-rtr.au-team.irpo

int ISP

ip address 172.16.5.2/28

ex

port te0

servise-instance te0/ISP

encapsulation untagged

connect ip interface ISP

ex

ex

int vl100

ip address 172.17.250.129/27

ex

port te1

service-instance te1/vl100

encapsulation untagged

rewrite pop 1

connect ip int vl100

ip route 0.0.0.0/0 172.16.5.1

ip name-server 77.88.8.8

show ip int brief 

так же стоит проверить ping isp-rtr

wr memory

_________________________

Hq-srv

vim /etc/net/ifaces/ens18/ipv4address – 172.17.250.66/26

vim /etc/net/ifaces/ ens18/ipv4route - default via 172.17.250.65

vim /etc/net/ifaces/ens18/resolv.conf <img width="210" height="41" alt="image" src="https://github.com/user-attachments/assets/e33e689a-485f-453c-9dfc-5f3099c03b24" />

vim /etc/net/ifaces/ens18/options <img width="284" height="154" alt="image" src="https://github.com/user-attachments/assets/4431068e-b846-4b5f-b6b5-dc8646249cc3" />

systemctl restart network

hostname hq-srv

useradd sshuser –u 1010

passwd sshuser

P@ssw0rd (два раза)

gpasswd –a sshuser wheel

vim/etc/openssh/sshd_config <img width="274" height="86" alt="image" src="https://github.com/user-attachments/assets/40d425a2-4f88-4fa3-858a-fa075208a7a9" />

vim /etc/openssh/banner 

systemctl restart sshd

ssh –p 2024 localhost

ssh –p 2024 sshuser@localhost

(для обоих серверов) vim /etc/apt/apt.conf <img width="520" height="43" alt="image" src="https://github.com/user-attachments/assets/33585e43-32bf-4e04-86c6-1c43b0ebd81a" />

_______________________________________________________________

Br-srv

vim /etc/net/ifaces/ens18/ipv4address – 172.17.250.130/27

vim /etc/net/ifaces/ens18/ipv4route - default via 172.17.250.129

vim /etc/net/ifaces/ens18/resolv.conf <img width="210" height="42" alt="image" src="https://github.com/user-attachments/assets/885473cf-1ed4-4dfe-b1df-749fa24ae654" />

vim /etc/net/ifaces/ens18/options <img width="284" height="154" alt="image" src="https://github.com/user-attachments/assets/b5bfb198-41d0-42d1-857b-e3a16051e828" />

systemctl restart network

hostname br-srv

useradd sshuser –u 1010

passwd sshuser 

P@ssw0rd (два раза)

gpasswd –a sshuser wheel

vim /etc/openssh/sshd_config <img width="259" height="81" alt="image" src="https://github.com/user-attachments/assets/193dfd63-2fa1-4320-9fa8-5973f7501a7f" />

vim /etc/openssh/banner  

systemctl restart sshd

ssh –p 2024 localhost

ssh –p 2024 sshuser@localhost

__________________________________________________________

hq-rtr/br-rtr (одинаковые команды)

en

conf t

username net_admin

password P@ssw0rd

role admin

wr memory

show users localdb

________________________________________________________

hq-rtr

interface tunnel.0

ip address 10.10.10.1/30

ip tunnel 172.16.4.2 172.16.5.2 mode gre

wr memory

show int tunnel.0

_________________________________________________________


bt-rtr

interface tunnel.0

ip address 10.10.10.2/30

ip tunnel 172.16.5.2 172.16.4.2 mode gre

wr memory

show int tunnel.0

__________________________________________________________

hq-rtr

en/conf t

router ospf 1

passive-interface default

no passive-interface tunnel.0

network 10.10.10.0/30 area 0

network 172.17.250.64/26 area 0

network 172.17.250.160/28 area 0

network 172.17.250.176/29 area 0

area 0 authentication

ex

int tunnel.0

ip ospf authentication-key P@ssw0rd

ex

wr memory

show ip ospf neighbor/show ip route ospf – для проверки

____________________________________________________________

br-rtr

en/conf t

router ospf 1

passive-interface default

no passive-interface tunnel.0

network 10.10.10.0/30 area 0

network 172.17.250.128/27 area 0

area 0 authentication

ex 

int tunnel.0

ip ospf authentication-key P@ssw0rd

ex

wr memory

show ip ospf neighbor/show ip route ospf – для проверки

после надо пропинговать сервера друг с другом

_________________________________________________________________

hq-rtr

en/conf t

int ISP

ip nat outside

int vl100

ip nat inside

int vl200

ip nat inside

int vl999

ip nat inside

ip nat pool HQ 172.17.250.65-172.17.250.254

ip nat source dynamic inside-to-outside pool HQ overload interface ISP

wr memory

_____________________________________________________________________

br-rtr

en/conf t

int ISP

ip nat outside

int vl100

ip nat inside

ip nat pool BR 172.17.250.129-172.17.250.254

ip nat source dynamic inside-to-outside pool BR overload interface ISP

пингуем сервера с ISP

show ip nat translations

пробуем прописать apt-get update на серверах

wr memory

________________________________________________________________________

hq-rtr

en/conf t

ip pool HQ-clients 172.17.250.162-172.250.174

dhcp-server

pool HQ-clients 1

mask 255.255.255.240

gateway 172.17.250.161

dns 172.17.250.66

domain-name au-team.irpo

ex

ex

int vl200

dhcp-server 1

ex

wr memory

show dhcp-server 1 detailed

show dhcp-server clients vl200 (после получения ip на hq-cli)

_______________________________________________________________________

hq-cli

заходим в машину, используем dhcp <img width="349" height="140" alt="image" src="https://github.com/user-attachments/assets/381a6664-ef70-40a8-9e2b-ae9568791a7f" />

_____________________________________________________________________

hq-srv

apt-get update && apt-get install bind –y

cd /etc/bind

mcedit named.conf – комментируем вторую строку <img width="433" height="85" alt="image" src="https://github.com/user-attachments/assets/48acd486-a99a-4d93-976a-5e23064d6493" />

mcedit options.conf (следующие параметры нужно изменить):

 listen-on { 172.17.250.66; };
 
 //listen-on-v6 { ::1; };
 
forwarders { 77.88.8.8; };

allow-query { any; }

allow-query-cache { any; }	

allow-recursion { any; }

mcedit local.conf 

zone “au-team.irpo”{

	type master;
 
	file “etc/bind/zone/db.au”;
 
}; <img width="505" height="199" alt="image" src="https://github.com/user-attachments/assets/f1e1c751-d84a-492c-9408-5971ef8573b0" />


zone “250.17.172.in-addr.arpa” {

	type master;
 
	file “etc/bind/zone/db.reverse”;
 
};

cd zone

ls

cp localhost db.au

mcedit db.au <img width="427" height="253" alt="image" src="https://github.com/user-attachments/assets/24bdcf3a-7334-4874-b943-e935116ec8db" />


cp db.au db.reverse

chown root:named db.*

mcedit db.reverse <img width="447" height="183" alt="image" src="https://github.com/user-attachments/assets/48a16787-00d9-4da6-93ae-0c1b821da094" />



named-checkzone au-team.irpo db.au

named-checkzone 250.17.172.in-addr.arpa db.reverse

sustemctl restart bind

cd /etc/net/ifaces/ens18/

mcedit resolv.conf <img width="244" height="52" alt="image" src="https://github.com/user-attachments/assets/3128fa1d-7ce2-4868-92e1-b1afda95676b" />

systemctl restart network

systemctl restart bind

host hq-srv

_______________________________________________________

Hq-rtr/br-rtr

en/conf t

ip name-server 172.17.250.66

ip domain-name au-team.irpo

ip domain-lookup

no ip name-server 77.88.8.8 (если есть в sh run)

do ping hq-srv

wr memory

____________________________________________________________

Br-srv hq-cli

Нужно зайти в resolv.conf

vim /etc/net/ifaces/ens18/resolv.conf - делаем так же <img width="244" height="52" alt="image" src="https://github.com/user-attachments/assets/ba9f54cf-e564-4372-8ca0-7834ae96ed82" />

____________________________________________________________

hq-cli/hq-srv/br-srv/ISP

timedatectl set-timezone Europe/Moscow

timedatectl

__________________________________________________________

Hq-rtr/br-rtr

ntp timezone utc+3

show ntp timezone

_____________________________________________________________





НОВАЯ ШПОРА РОМЫ 2026 ИЮНЬ:

<img width="781" height="280" alt="image" src="https://github.com/user-attachments/assets/2f744095-2290-4218-8e54-a43685f6b324" />

ТАБЛИЦА:
<img width="505" height="739" alt="image" src="https://github.com/user-attachments/assets/c711b5fa-0572-465b-94b9-2467e16db59f" />


Модуль 1 — Быстрый готовый конфиг с командами без лишнего 🔥
#с таким значком ниже будут комментарии для объяснения где надо менять айпи и какой именно айпи. Впринципе чтоб не лохануться смотри всегда на таблицу и сверяй со своим вариантом
Заточен под реальный стенд на Proxmox в колледже
🐧 ISP

hostnamectl set-hostname ISP;exec bash

vim /etc/net/ifaces/ens18/options

BOOTPROTO=dhcp

TYPE=eth

vim /etc/net/ifaces/ens18/resolv.conf

search au-team.irpo

nameserver 77.88.8.8   #это днс яндекса, этот ip не меняем на экзамене

vim etc/net/sysctl.conf – поменять 0 на 1

mkdir /etc/net/ifaces/ens19

mkdir /etc/net/ifaces/ens20

vim/etc/net/ifaces/ens19/options 

vim /etc/net/ifaces/ens20/options

vim /etc/net/ifaces/ens19/ipv4address 

vim /etc/net/ifaces/ens20/ipv4address

systemctl restart network

_____

apt-get update

apt-get install tzdata -y

timedatectl set-timezone Europe/Moscow

____

apt-get install iptables -y

iptables -t nat -A POSTROUTING -o ens18 -j MASQUERADE

iptables-save >> /etc/sysconfig /iptables

systemctl enable --now iptables

systemctl restart network

iptables -t nat -L -n -v

reboot


🍃 HQ-RTR
en

conf t

hostname hq-rtr.au-team.irpo

ip domain-name au-team.irpo

int ISP

ip address 172.16.1.2/28       #первый ip на hq-rtr, который ведет на isp

ip nat outside

ex

port te0

servise-instance ISP

encapsulation untagged

connect ip interface ISP

int 100

ip nat inside

ip address 192.168.2.1/27       #айпи vlan 100

int 200

ip address 192.168.2.33/27      #айпи vlan 200

ip nat inside

int 999

ip address 192.168.2.65/29       #айпи vlan 100

ip nat inside

port te1 

service-instance 100

encapsulation dot1q 100

rewrite pop 1

connect ip int 100

ex

service-instance 200

encapsulation dot1q 200

rewrite pop 1

connect ip int 200

ex

service-instance 999

encapsulation dot1q 999

rewrite pop 1

connect ip int 999

ip route 0.0.0.0/0 172.16.1.1    #ip isp’а, который ведет на hq-rtr (тоесть ens19 интерфейс в табличке сверху)

____

ntp timezone utc+3

username net_admin
password P@ssw0rd
role admin

____

ip pool dhcp 1

range 192.168.2.34-192.168.2.62   #тут диапозон адресов для vlan 200, который должен начинаться с адреса hq-cli

##########################################################################

Можно скипунть то что в решетках тут, кроме тех кто ↓

Ниже небольшая обьяснялка этого момента кто запутался и не пон ничего 

На ipmeter.ru, когда разбили свой адрес на 3 vlan’a

<img width="480" height="309" alt="image" src="https://github.com/user-attachments/assets/d03691ff-fcb1-4a25-a36c-29795548becb" />

Берем диапозон адресов от подсети vlan 200 и добавляем к нему однёрку (тоесть адрес hq-cli, если вы считали свою табличку так, как в этом примере показано)

##########################################################################

dhcp-server 1

pool dhcp 64

mask 255.255.255.224	#маска ip адреса vlan 200

gateway 192.168.2.33   #ip адрес vlan 200

dns 192.168.2.2     #ip адерес hq-srv

domain-name au-team.irpo

ip domain-lookup

interface 200

dhcp-server 1

____

ip nat pool nat 192.168.2.1-192.168.2.30,192.168.2.33-192.168.2.62,192.168.2.65-192.168.2.70      # тут с того же ipmeter.ru берем все диапазоны адресов подсетей и пишем сюда через запятую    <img width="80" height="187" alt="image" src="https://github.com/user-attachments/assets/4e2eed0d-d819-4a0a-9374-dfc8ed8709a8" />

ip nat source dynamic inside-to-outside pool nat overload interface ISP

____


interface tunnel.1

ip add 10.10.10.1/30   #ip адрес тунеля может быть любой, советую использовать такой же как тут, но если другой, то он должен быть вроде как всегда на /30 маске (пример: 192.168.5.1/30)

ip tunnel 172.16.1.2 172.16.2.2 mode gre   #ip адреса hq-rtr и br-rtr соответственно (по порядку), которые ведут на isp

ip ospf authentication-key P@$$word

____

router ospf 1

passive-interface default

no passive-interface tunnel.1

network 10.10.10.0/30 area 0  #адрес подсети туннеля. (для примеров ниже, если сам не можешь высчитать адрес подсети, то также на ipmeter при разбивке адресов написаны адреса подсети)

network 192.168.2.0/27 area 0  #адрес подсети vlan 100

network 192.168.2.32/27 area 0  #адрес подсети vlan 200

network 192.168.2.64/29 area 0   #адрес подсети vlan 999

area 0 authentication

write memory

show ip int brief

🍃 BR-RTR

en

conf t

hostname br-rtr.au-team.irpo

ip domain-name au-team.irpo

_____

int ISP

ip address 172.16.2.2/28  #ip адрес br-rtr, который ведет на isp

ip nat outside

ex

port te0

service-instance ISP

encapsulation untagged

connect ip interface ISP

int 100

ip address 192.168.3.1/28  #ip адрес br-rtr, который ведет на br-srv

ip nat inside

ex

port te1

service-instance 100

encapsulation untagged

rewrite pop 1

connect ip int 100

ip route 0.0.0.0/0 172.16.2.1  #ip isp’а, который ведет на br-rtr (тоесть ens20 интерфейс в табличке сверху)

ip domain-lookup

show ip int brief 

____

ntp timezone utc+3

username net_admin

password P@ssw0rd

role admin

____

ip nat pool nat 192.168.3.1-192.168.3.14  #диапазон адресов подсети br-srv, который находится на br-rtr ( тут тоже если запутался, то с ipmeter.ru данные бери)  <img width="511" height="237" alt="image" src="https://github.com/user-attachments/assets/c939303f-e88e-4936-b389-c2be927a6590" />


ip nat source dynamic inside-to-outside pool nat overload interface ISP

_____

interface tunnel.1

ip add 10.10.10.2/30   # ip адрес туннеля, но уже другого устройства. Просто добавляем единичку в конце первого адреса туннеля

ip tunnel 172.16.2.2 172.16.1.2 mode gre   ##ip адреса br-rtr и hq-rtr соответственно (по порядку), которые ведут на isp

ip ospf authentication-key P@$$word

______

router ospf 1

passive-interface default
no passive-interface tunnel.1

network 10.10.10.0/30 area 0.0.0.0   #адрес подсети туннеля

network 192.168.3.0/28 area 0.0.0.0    #адрес подсети br-srv на br-rtr

area 0 authentication

write memory

🐧 HQ-CLI

hostnamectl set-hostname hq-cli.au-team.irpo;exec bash

systemctl restart network

timedatectl set-timezone Europe/Moscow


🐧 BR-SRV

hostnamectl set-hostname br-srv.au-team.irpo;exec bash

_____


vim /etc/net/ifaces/ens18/options

BOOTPROTO=static

TYPE=eth

CONFIG_WIRELESS=no

SYSTEMD_BOOTPROTO=static

CONFIG_IPV4=yes

DISABLED=no

NM_CONTROLLED=no

SYSTEMD_CONTROLLED=no

vim /etc/net/ifaces/ens18/resolv.conf

search au-team.irpo

nameserver 192.168.2.2   #ip адрес hq-srv

echo 192.168.3.2/28 > /etc/net/ifaces/ens18/ipv4address       #ip адрес br-srv

echo default via 192.168.3.1 > /etc/net/ifaces/ens18/ipv4route   #ip адрес br-srv на br-rtr, или же шлюз br-srv

systemctl restart network

timedatectl set-timezone Europe/Moscow

____

useradd -s /bin/bash -u 2026 sshuser

echo "sshuser:P@ssw0rd" | chpasswd

gpasswd -a sshuser wheel

echo 'sshuser ALL = (root) NOPASSWD: ALL' >> /etc/sudoers

_____

vim /etc/openssh/sshd_config

Port 2026

MaxAuthTries 2

Banner /etc/openssh/banner

AllowUsers sshuser

echo Authorized access only > /etc/openssh/banner

systemctl restart sshd

vim /etc/apt/apt.conf

______

Acquire::http::Proxy "http://10.0.21.52:3128/"     #это прокси колледжа, его не меняем

Acquire::https::Proxy "http://10.0.21.52:3128/"


🐧 HQ-SRV

hostnamectl set-hostname hq-srv.au-team.irpo;exec bash

_____ 

vim /etc/net/ifaces/ens18/options

BOOTPROTO=static

TYPE=eth

CONFIG_WIRELESS=no

SYSTEMD_BOOTPROTO=static

CONFIG_IPV4=yes

DISABLED=no

NM_CONTROLLED=no

SYSTEMD_CONTROLLED=no


echo 192.168.2.2/27 > /etc/net/ifaces/ens18/ipv4address  #ip адрес hq-srv

echo default via 192.168.2.1 > /etc/net/ifaces/ens18/ipv4route   #ip адрес vlan 100 на hq-rtr или же шлюз hq-srv

vim /etc/net/ifaces/ens18/resolv.conf

search au-team.irpo

nameserver 77.88.8.8  #эт тоже днс яндекса, его не меняем

systemctl restart network

timedatectl set-timezone Europe/Moscow

______

useradd -s /bin/bash -u 2026 sshuser

echo "sshuser:P@ssw0rd" | chpasswd

gpasswd -a sshuser wheel

echo 'sshuser ALL = (root) NOPASSWD: ALL' >> /etc/sudoers

_____

vim /etc/openssh/sshd_config

Port 2026

MaxAuthTries 2

Banner /etc/openssh/banner

AllowUsers sshuser

echo Authorized access only > /etc/openssh/banner

systemctl restart sshd

______

apt-get update && apt-get install dnsmasq -y

echo 'OPTIONS=""' > /etc/sysconfig/dnsmasq

systemctl restart network

cat <<'EOT' > /etc/dnsmasq.conf

no-resolv

no-poll

no-hosts

пробел

server=77.88.8.7   #эти три строчки,

server=195.208.4.1  #не меняем

server=8.8.8.8    #это тоже адреса днс ресурсов

пробел

cache-size=1000

all-servers

no-negcache

пробел

host-record=hq-rtr.au-team.irpo,192.168.2.1  #ip адрес vlan 100 на hq-rtr

host-record=hq-srv.au-team.irpo,192.168.2.2  #ip адрес hq-srv

host-record=hq-cli.au-team.irpo,192.168.2.34  #ip адрес hq-cli

host-record=br-rtr.au-team.irpo,192.168.3.1  #ip адрес br-srv на br-rtr

host-record=br-srv.au-team.irpo,192.168.3.2  #ip адрес br-srv

пробел

address=/docker.au-team.irpo/172.16.1.1   #ip адрес isp’a, которые смотрит на hq-rtr

address=/web.au-team.irpo/172.16.2.1   #ip адрес isp’a, которые смотрит на br-rtr

#можно добавить строки address=/br-rtr.au-team.irpo/192.168.2.1 и address=/br-srv.au-team.irpo/192.168.2.2



EOT

systemctl enable --now dnsmasq

______

vim /etc/apt/apt.conf

Acquire::http::Proxy "http://10.0.21.52:3128/"    #прокси колледжа, не меняем адреса

Acquire::https::Proxy "http://10.0.21.52:3128/"

