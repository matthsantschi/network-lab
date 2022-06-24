# Einleitung
Alle Arbeiten wurden im dafür eingerichteten BFH-Lab erledigt. Das Lab hat folgende
Architektur:

![Architektur Lab](images/lab-architecture.png)


Zugangsdaten zum Lab:

username: netlab

password: hv$mast3r

[https://147.87.85.239](https://147.87.85.239/)!

Die Konfigurationen wurden wo möglich nicht via Webadmin sondern via
Konfigurations-Datei erstellt.  

# Exercise 1 Static Routing
## Task 2
### Aufgabe
Now start the "router" VM and configure the router interfaces according to the guidelines1 for your group:
"ens192" → uplink
"ens224" → servers
"ens256" → clients
See "S22.L2 – Lab Networks and Names" → "Routing and DHCP information". For the configuration you can use the administration tool "webmin".
### Erarbeitung

Die Interface-Konfiguration kann im File  `/etc/network/interfaces` vorgenommen werden: 

![screenshot-interfaces-task2](images/screenshot-interfaces-task2.png)
auto ens192 bedeutet, dass das Interface beim Befehl `ifup -a` automatisch 
aktiviert wird. (Task 3) 


## Task 4
### Aufgabe
Now configure the "Default Route" of your router according to the guidelines.
### Erarbeitung
Die "Default Route" kann mit folgendem Befehl zur Routing Tabelle hinzugefügt werden:
```
ip route add default via 147.87.80.250
```

## Task 5
### Aufgabe
In order for your router to resolve other machines by name, you must next configure its DNS resolver. (...)
### Erarbeitung

Für diesen Task wurde die Datei `resolv.conf` editiert

![dns configuration](images/screenshot-task5.png)

Die finale Konfiguration von `interfaces.d` sieht wir folgt aus:

![image-20220504211937773](images/interfaces.d_final.png)

## Task 6

Auch nach einem Reboot kann der router die Webseite based.cooking erreichen. 

![image-20220601200914798](images/ping-task6.png)

# Exercise 2 DHCP Server Configuration
In diesem Teil geht es darum, den DHCP-Server auf dem Router zu konfigurieren, 
so dass die Clients dynamische Adressen erhalten. 
## Task 2

### Aufgabe
Configure here the options "Domain name", "DNS servers" with the domain and the DNS servers according to "S22.L2 – Lab Networks and Names" → "DNS information") for your group.
Set the "Default lease time" and the "Maximum lease time" to small values (e.g. 200 and 300 secs) so that later on DHCP traffic is generated on your networks and you can record and analyze it with "wireshark".

### Erarbeitung

Die Einstellungen konnten im File `/etc/dhcp/dhcpd.conf` vorgenommen werden:

![image-20220601202332997](images/dhcp-dns-configuration.png)

## Task 3
Die Restrikion, auf welchen Interfaces der Router DHCP-Adressen anbietet, kann in folgendem File getätig werden:
> /etc/default/isc-dhcp-server

## task 4
Hier hatten wir Probleme, dass der DHCP-Dienst nicht startete. Der Befehl `journalctl -xe` 
zeigt das Journal alles Systemd-Dienste. Dies hat uns aufgezeigt, dass unsere Konfiguraiton in
`/etc/dhcp/dhcpd.conf` fehlerhaft war. 
Nach einer Korrekur, konnte der Dienst gestart werden. 
![image-20220601211651778](images/dhcp-service-running.png)
Ausschnitt aus `/etc/dhcp/dhcpd.conf`:
![image-dhcpd-subnet](images/dhcp-subnet-config.png)
Nun können die Clients Adressen in der Rage 147.87.89.210  - 222 / 28 beziehen. 

# exercise 3
## task 1
Der folgende Screenshot zeigt ein Wireshark ausschnitt, welcher ein DHCP-Lease Ablauf zeigt 
zwischen dem Router(DHCP-Server) 147.87.89.209 und dem Client
147.87.89.209 
![wireshark dhcp handshake](images/wireshark-dhcp.png)


## task 2/3

![image-20220601212505113](images/ipconfig-windows-machine.png)
Der Client02 hat eine korrekte IP erhalten.
## task 4
Die Internet-Verbindung steht, es kann normal gesuft werden. 
![image-20220601212632982](images/windows-ping.png)
