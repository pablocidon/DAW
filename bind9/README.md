#  SERVIDOR DNS
*Instalación de un servidor de DNS*

[Acceso al servidor](http:192.168.1.150)
- [ ] Instalación de bind
```bash
sudo apt-get update
sudo apt-get install bind9
```
- [ ] Configuración del servidor DNS

*Fichero /etc/bind/named.conf.local*
```bash
zone "pablo.local" {
type master;
file "/etc/bind/db.pablo.local";
};
zone "1.168.192.in-addr.arpa"{
type master;
file /etc/bind/db.1.168.192.in-addr.arpa";
};
```
*Chequear el fichero*
```bash
 sudo named-checkconf /etc/bind/named.conf.local
```
*Reiniciar el servicio bind9*
```bash
 sudo service bind9 restart
```
- [ ] Configuración de la zona de resolución de nombres directa
```bash
sudo cp /etc/bind/db.0 /etc/bind/db.pablo.local
```
*Editar el fichero "db.tunombre.local"*
```bash
@       IN      SOA     PCB-ED.pablo.local. root.localhost. (
                              1         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      PCB-ED.pablo.local.
PCB-ED  IN      A       192.168.1.150
PC-PC   IN      A       192.168.1.101
```
*Chequear el fichero*
```bash
sudo named-checkzone pablo.local /etc/bind/db.pablo.local
```
- [ ] Configuración de la zona de resolución de nombres inversa
```bash
sudo cp /etc/bind/db.0 /etc/bind/db.1.168.192.in-addr.arpa
```
*Editar el fichero "db.1.168.192.in-addr.arpa"*
```bash
@       IN      SOA     PCB-ED.pablo.local. root.localhost. (
                              1         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      PCB-ED.pablo.local.
101     IN      PTR     PC-PC.pablo.local.
150     IN      PTR     PCB-ED.pablo.local.
```
*Chequear el fichero*
```bash
sudo named-checkzone pablo.local /etc/bind/db.1.168.192.in-addr.arpa
```
*Reiniciar el servicio*
```bash
sudo service bind9 restart
```
- [ ] Configurar reenviadores

*Editar el fichero "/etc/bind/named.conf.options"*
```bash
sudo nano /etc/bind/named.conf.options

forwarders {
  8.8.8.8;
};
```
*Reiniciar el servicio*

```bash
sudo service bind9 restart
```
- [ ] Configuración del fichero "/etc/network/interfaces"
```bash
sudo nano /etc/network/interfaces
```
*IP del propio servidor como DNS*
```bash
dns-nameservers 192.168.1.150
```
*Agregar a un dominio*
```bash
dns-domain pablo.local
```
*Reiniciar el sistema*
```bash
sudo reboot now
```
*Comprobar el archivo "/etc/resolv.com"*
```bash
nameserver 192.168.1.150
search pablo.local
```
- [ ] Creación de alias

*Editar el fichero "/etc/bind/db.pablo.local"*
```bash
ftp     IN      CNAME   PCB-ED.pablo.local.
www     IN      CNAME   PCB-ED.pablo.local.
```
*Chequear el fichero*
```bash
sudo named-checkzone pablo.local /etc/bind/db.pablo.local
```
*Reiniciar el servicio*
```bash
sudo service bind9 restart
```
