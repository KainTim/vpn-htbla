# BENÖTIGTE PACKAGES INSTALLIEREN
Bitte der Anleitung folgen, die der genutzten Distribution entspricht!


## Fedora

Mit diesem Befehl werden die Dependencies für die l2tp Verbindung unter Fedora/RHEL und Derivaten(Nobara) installiert  .

```
sudo dnf install NetworkManager-l2tp NetworkManager-strongswan
```

Mit diesem Befehl entfernen wir libreswan, weil network-manager sonst strongswan nicht verwendet.  
Wenn libreswan benötigt wird, diesen Befehl nicht ausführen!  

```
sudo dnf remove libreswan
```

Mit diesen Befehlen werden die benötigten Kernel-Module entblockiert

```
sudo sed -e '/blacklist l2tp_netlink/s/^b/#b/g' -i /etc/modprobe.d/l2tp_netlink-blacklist.conf
sudo sed -e '/blacklist l2tp_ppp/s/^b/#b/g' -i /etc/modprobe.d/l2tp_ppp-blacklist.conf
```

Nun kann mit der Sektion "Verbindung Herstellen" weitergemacht werden.


## Debian

Unter Debian basierten Distributionen müssen als erstes alle Pakete zu strongswan und l2tp entfernt werden.

```
apt purge libstrongswan
apt purge xl2tpd
```

Das erfolgreiche Entfernen kann mit den folgenden Befehlen überprüft werden. Beide Befehle sollten kein Ergebnis liefern.

```
apt list --installed | grep swan
apt list --installed | grep l2tp
```

Mit diesem Befehl werden die Dependencies für die l2tp Verbindung unter Debian und Derivaten(Ubuntu, Linux Mint, Kali Linux) installiert.

```
apt install network-manager-l2tp network-manager-strongswan
```

Nun kann mit der Sektion "Verbindung Herstellen" weitergemacht werden.


## Arch

Mit diesem Befehl werden die Dependencies für die l2tp Verbindung unter Arch Linux und Derivaten(Manjaro, Endevour) installiert.

```
sudo pacman -S networkmanager-l2tp networkmanager-strongswan
```


## openSuSE Tumbleweed

Mit diesem Befehl werden die Dependencies für die l2tp Verbindung unter OpenSuSE Tumbleweed und OpenSuSE Leap installiert.

```
sudo zypper install NetworkManager-l2tp strongswan-nm
```

Nun kann mit der Sektion "Verbindung Herstellen" weitergemacht werden.


# VERBINDUNG ERSTELLEN
Dieser Befehl lädt die Konfigurationsdatei von Github herunter und speichtert sie im aktuellen Verzeichnis unter htbla-vpn.conf.

```
curl https://raw.githubusercontent.com/KainTim/vpn-htbla/main/htbla-vpn.conf > htbla-vpn.conf
```

Mit diesem Befehl importieren wir die vorher heruntergeladene Datei.

```
nmcli connection import type l2tp file htbla-vpn.conf
```

Dieser Befehlt setzt den Benutzernamen in der VPN-Verbindung auf den eingegebenen Wert (NachnameVXXXXX).

```
nmcli connection modify HTBLA-VPN vpn.user-name <USERNAME HIER EINGEBEN>
```

Mit diesem Befehl wird das Passwort in der VPN-Verbindung gesetzt(Passwort beim anmelden an den Schulrechnern).

```
nmcli connection modify HTBLA-VPN vpn.secrets "ipsec-psk = htlgrieskirchen, password = <PASSWORT HIER EINGEBEN>"
```



# VERBINDUNG AKTIVIEREN
Dieser Befehl aktiviert die Verbindung nun schlussendlich.  
Das kann später auch in einer GUI gemacht werden, beim ersten mal sollte es aber über das Terminal gemacht werden um zu sehen ob alles funktioniert.
Dieser Befehl funktioniert nicht in der Schule, da man nicht im selben Netzwerk sein kann.

```
nmcli connection up HTBLA-VPN
```



# AUFRÄUMEN

Mit diesem Befehl löschen wir die zuvor heruntergeladene Konfigurationsdatei, die nun nicht mehr benötigt wird.

```
rm htbla-vpn.conf
```


# S-LAUFWERK VERBINDUNG

Das S-Laufwerk kann bei aktiver VPN-Verbindung nun unter 10.10.0.3/Daten in den meisten File managers erreicht werden.
Beim eingeben des Benutzernamens muss aufgepasst werden, da er in der Form 

```htl\<Benutzername>``` 

eingegeben werden muss.
