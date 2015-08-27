# dhcplab1

Ni ska skapa enklast möjliga dhcpserver som ger ut ip-adresser åt en klient.  För detta har jag skrivit en Vagrantfile [multimachine](https://docs.vagrantup.com/v2/multi-machine/ ) som skapar  två maskiner *happyclient* och *happyserver*.

Förutsättningen för att det här ska fungera är att ni har följt mina tidigare intruktioner och installerat linux, virtualbox och vagrant.

## Ladda ner  Vagrantfile

börja med att clona (ladda ner det här projektet)  öppna en terminal
gå till eran lab folder ( **Hint:** cd)

använd följande magsiska kommando som laddar ner projektet och skapar en folder dhcplab

	$ git clone  https://github.com/ntisodbojo/dhcplab1.git

gå  in katalogen

	$ cd dhcplab

nu är vi klar för att starta maskinerna

	$ vagrant up

för att ssh:a in i maskinerna måste ni nu lägga till namnet. Börja med att ssh in i happyserver

	$ vagrant ssh happyserver


## Installera  [dhcpservern](https://www.isc.org/downloads/dhcp/) 

	vagrant@happyserver:~$  sudo apt-get install isc-dhcp-server 

Nu ska  ni konfigurera denm ni hittar en manual [här](https://help.ubuntu.com/community/isc-dhcp-server) eller så följer ni mitt exempel.

börja med att öppna konfigurationsfilen, vi kommer att använde an editor som heter nano

	vagrant@happyserver:~$ sudo nano /etc/dhcp/dhcpd.conf
	
Titta gärna snabbt igenom filen, kom ihåg att alla rader som börjar med `#`  är kommentarer(exempel)

Jag valde en väldig lätt konfiguration.

	subnet 10.11.12.0 netmask 255.255.255.0 { 
	range 10.11.12.100 10.11.12.200; 
	}

Allt annat fick vara default, ni kan skriva era subnet utdelning längst ner eller var ni tycker det passar

Sedan måste vi tala om vilket **interface** dhcp servern ska lyssna på det gör vi genom att öppna 

	sudo nano /etc/default/isc-dhcp-server

längst ner hittar ni INTERFACES lägg till eth1
	
	INTERFACES="eth1"

Nu är konfigurationen klar och det är bara att starta om dhcp-servern

	 vagrant@happyserver:~$ sudo service isc-dhcp-server restart
	 isc-dhcp-server stop/waiting
	 isc-dhcp-server start/running, process 8872


## Konfigurera klienten att använda dhcpservern

öppna en ny terminal gå till dhcplab foldern, starta klienten med 

	$ vagrant ssh happyclient

vi har ingen konfiguration för nätverkskortet eth1, nätverkskorten konfigureras i en fil som heter interfaces öppna den för editering

	vagrant@happyclient:~$ sudo nano /etc/network/interfaces

jag la till följande 

	auto eth1 
	iface eth1 inet dhcp

avsluta/spara filen och start om nätverkskortet med

	vagrant@happyclient:~$ sudo ifdown eth1 && sudo  ifup eth1

ni borde se en hur klient kommunicerar med dhcpserver och får en ip-adress.

Mer teori tar vi på lektionen

**Lycka till!**







