# Prikazy
## Obsah



## Zakladni prikazy
- **en** (enable) - zapne privilegovany rezim
- **conf t** (configure terminal) - zapne global configuration mode
- **reload** - restart zarizeni
- **sh running-config** (show running-config) - ukaze momentalne bezici config
- **erase startup-config** - vymaze config
- **hostname <nazev>** - nastavi nazev zarizeni
- **int <port>** (interface <port>) - vstoupi do konfiguracniho modu pro konkretni interface (port)
- **no <jakykoliv_prikaz>** - pokud neco nastavime spatne, muzeme zrusit prikaz tim, ze ho zopakujeme, ale pridame pred prikaz "no"
### Prikazy pouzivane pro nastaveni portu
- **no sh** (no shutdown) - no shutdown
- **ip address** - nastavi IP adresu na dany interface

## IP route
- IP route se pouziva pri statickem smerovani
- Je nutno kazdy router naucit kazdou dalsi podsit krom tech, ktere ma hned vedle sebe (ty zna)
- **ip route <IP site> <maska site> <IP portu>**
	- za IP portu doplnujeme IP posledniho znameho mista, pred danou podsiti
	- ```ip route 10.0.1.0 255.255.255.128 10.0.2.1```

## OSPF
- OSPF se pouziva pri dynamickem smerovani
```
router ospf 1
network <IP site> <Wildcard maska> area 0
```
- Wildcard maska ma opacne usporadani 1 a 0 oproti masce site
	- Maska site: 255.255.255.0
	- Wildcard maska: 0.0.0.255
