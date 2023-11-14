## Windows iso
*Zde najdeme vsechny  iso soubory, pokud nejsou zahrnuty v nasi win_srv slozce*

\\hp82\win_srv

## IP Adresy
*Muzou byt nastaveny podle potreby*
**SRV22-DC: 172.16.0.1**

**Win81**: 172.16.0.2

**Win19**: 172.16.0.3

**Win11**: 172.16.0.4

### Jak nastavovat IP?
**Adress**: *IP Stroje - viz prehled vyse*

**Subnet Mask**: Nastavi se automaticky po zmacknuti TAB

**Default Gateway**: *IP SRV22-DC*

**DNS Server**: *IP SRV22-DC*


## Commands
**gpupdate /force** - resetovani policies

##Jak nastavit domovsky adresar pro kazdeho uzivatele
###1. Sdileni
**Share name**: <nazev_slozky>**$**__
Nastavit opravneni pro **Authenticated Users**, aby mohli psat a cist. A odstranit **Everyone**, ulozit  cesku \\<nazev_pc>\<nazev_slozky>__
V security jit do **advanced security settings**. Disable inheritance a smazat **Users (Read and Execute)**.

V SRV22-DC nastavit v users zalozce **Profile** home folder a nezapomenout na cestu ke slozce a za ni **\%username%**. 

**Po nastaveni je dulezite se odhlasit a prihlasit na clenskem pocitaci**