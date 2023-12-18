## Windows iso
*Zde najdeme vsechny  iso soubory, pokud nejsou zahrnuty v nasi win_srv slozce*<br />
\\hp82\win_srv

## IP Adresy
*Muzou byt nastaveny podle potreby*
- **SRV22-DC: 172.16.0.1**
- **SRV-22**: 172.16.0.2
- **Win81**: 172.16.0.3
- **Win19**: 172.16.0.4
- **Win11**: 172.16.0.5

### Jak nastavovat IP?
- **Adress**: *IP Stroje - viz prehled vyse*
- **Subnet Mask**: Nastavi se automaticky po zmacknuti TAB
- **Default Gateway**: *IP SRV22-DC*
- **DNS Server**: *IP SRV22-DC*


## Commands
**gpupdate /force** - resetovani policies

## Overeni typu uctu uzivatele
WIN + PAUSE -> Advanced system settings -> User Profiles (settings)

## Jak nastavit domovsky adresar pro kazdeho uzivatele
### 1. Sdileni
- **Share name**: <nazev_slozky>**$**
- Nastavit opravneni pro **Authenticated Users**, aby mohli psat a cist. A odstranit **Everyone**, ulozit  cesku \\<nazev_pc>\<nazev_slozky>
### 2. Nastaveni prav
- V security jit do **advanced security settings**. Disable inheritance a smazat **Users (Read and Execute)**.
### 3. SRV22-DC
- V SRV22-DC nastavit v users zalozce **Profile** home folder a nezapomenout na cestu ke slozce a za ni **\%username%**. 
- **Po nastaveni je dulezite se odhlasit a prihlasit na clenskem pocitaci**

## Jak omezit cas prihlaseni a stroj pro uzivatele 
### 1. Cas prihlaseni
- Na SRV22-DC si otevrit podrobnosti uzivatele, ktereho chceme upravovat.
- Otevrit si **Account** zalozku a nasledne **Logon Hours...* 
- Nastavime Logon Hours a ulozime :)

### 2. Omezeni stroje pro uzivatele
- Na SRV22-DC si otevrit podrobnosti uzivatele, ktereho chceme upravovat.
- Otevrit si **Account** zalozku a nasledne **Log On To...* 
- Nastavime pocitac a ulozime :)

## Jak nastavit ukladani profilu / Jak vytvorit roaming uzivatele
### 1. Vytvoreni slozky
- Vytvorime slozku **Profiles** na Srv19 stejne, jako jsme to udelali u domovskeho adresare. 
- Nastavime permise stejne jako u domovskeho adresare. 
### 2. Nastaveni uzivatelskych profilu v SRV22-DC
- Na SRV22-DC si otevrit podrobnosti uzivatele, ktereho chceme upravovat.
- Otevrit si **Profile** zalozku a nasledne vepsat **Profile Path**
- V profile path by melo by: **\\<nazev_serveru>\<nazev_slozky>\%username%**

## Jak nastavit spolecny adresar (sklad)
### 1. Prejdeme na Srv19 nebo jiny pomocny server 
- Na disku C: vytvorime adresar **"Sklad" a nasdilime ho**
- Pri nastavovani permisi nechame **Everyone - full control**
### 2. Nastaveni Skladu jako Disk S:
- Vytvorime logovaci script na Srv22-DC
- net use S: \\Srv19\Sklad
- Vlozime tento .bat soubor do C:\Windows\SYSVOL\sysvol\dom22.local\scripts
### 3. Upraveni slozky s userama
- Oznacime vsechny usery a otevreme properties
- Prejdeme do karty **Profile** a do logon scripts napiseme nazev naseho scriptu

## Jak vytvorit mandatory uzivatele
### 1. Vytvorit roaming uzivatele
- Vytvorime roaming uzivatele
- Nebo skopirujeme roaming uzivatele
- Musime mit funkcni profiles slozku
### 2. SRV22 nastaveni / nastaveni slozky v Profiles
- Prihlasime se za nove vytvoreneho roaming uzivatele, aby se nam v Profiles vytvorila slozka tohoto uzivatele
- Potrebujeme pristoupit do uzivatelske slozky vytvorene automaticky - napr. "b3.V2"
  - Nastavime skupinu Administrators jakozto **ownera teto slozky**, abychom do ni mohli pristoupit
      - Zaskrtneme, aby se Administrator nastavil jako owner vsech podslozek a souboru
  - Nastavime userovi **3 permise** - read, read & execute, list
      - Refreshneme permise v advanced 
- Otevreme soubor NTUSER.DAT, ktery je hidden soubor, cili musime **zobrazit i hidden soubory** a prejit do options -> view -> odskrtneme hide protected files
- Prejmenujeme tento soubor na NTUSER.MAN
- Otestujeme toto nastaveni prihlasenim se za uzivatele

## Jak vytvorit logy prihlaseni
