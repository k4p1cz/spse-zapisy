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
- **Win10**: 172.16.0.6

### Jak nastavovat IP?
- **Adress**: *IP Stroje - viz prehled vyse*
- **Subnet Mask**: Nastavi se automaticky po zmacknuti TAB
- **Default Gateway**: *IP SRV22-DC*
- **DNS Server**: *IP SRV22-DC*


## Commands
**gpupdate /force** - resetovani policies

## Zmirneni naroku na heslo
- Na Srv22-DC si otevreme Server Manager a prejdeme do **Tools->Group Policy Management**
- V levem menu si rozklikneme nas forrest -> domenu a rozklikneme si *Default Domain Policy** - pravym na to klikneme a dame **edit**
- Najdeme si cestu - Policies/Windows Settings/Security Settings/Account Policies/Password Policy
- Zmenime **"Minimum password length"** na **3**
- Zmenime **"Password must meet complexity requeirements"** na **disabled**
- Pomoci [commandu](https://github.com/k4p1cz/spse-zapisy/blob/main/POS/Cviceni/_obecne_utils.md#commands-1) na resetovani policies zadaneho do cmd.exe se hned projevi zmeny
- Otestujeme a mame hotovo :)

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

## Jak vytvorit skupinu a pridat do ni uzivatele
### 1. Vytvorit skupinu
- New -> Group - nastavit nazev skupiny - zbytek nastavovat nemusime
### 2. Pridat uzivatele do groupy
- Poklikat na uzivatele -> Member Of -> Add -> "Nazev skupiny" -> OK

## Jak vytvorit skupinovy mandatorni profil
- Vytvorime skupinu podle [tohoto](https://github.com/k4p1cz/spse-zapisy/blob/main/POS/Cviceni/_obecne_utils.md#jak-vytvorit-skupinu-a-pridat-do-ni-uzivatele) navodu a defaultniho uzivatele, ktereho do teto skupiny pridam
### 1. Zkopiruju si "Default Profile" do "\\Srv22\Profiles$\" 
- Prihlasim se na klientsky stroj a dam **WIN+PAUSE** -> **Advanced system settings** a prejdu do nastaveni "User profiles"
- Najdu si default profile a dam **Copy To** 
- Copy profile to musi byt nastaveno na nasdilenou slozku na Srv22 - v nasem pripade je to slozka **Profiles** (\\Srv22\Profiles$\<nazev_slozky>)
- nazev_slozky si nastavim podle skupiny a verze Windowsu (pro WIN11 -> **<nazev_slozky>.V6**
- Permise si nastavim na groupu, kterou jsme vytvorili v predchozim kroku - pokud ji nemuzeme najit, zmenime si **Object types** na **Group**
- Zkontrolujeme si, ze se do tohoto adresare muzeme podivat 
### 2. Nastaveni Srv22-DC
- Prejdu do nastaveni usera, pro ktereho toto chci nastavit
- Jdu do karty "Profiles" a do profile path zadavam - **\\Srv22\<nazev_slozky>$\<nazev_defaultniho_profilu>** - Nazev defaultniho profilu musi byt bez koncovky .V<x>
### 3. Nastaveni defaultniho profilu
- Prihlasim se pod defaultnim profilem a nastavim si UI a obsah tohoto profilu
- **NESMIM ZAPOMENOUT SE ODHLASIT!**
### 4. Srv22
- Najdu si slozku Defaultniho profilu a upravim tento profil na mandatory - viz. nastaveni mandatory uctu
- Odstranim "Write" permisi pro slozku Defaultniho profilu a nastavim toto pro vsechny podslozky
### 5. Otestuju
- Vytvorim si koncoveho uzivatele kopirovanim Defaultniho mandatorniho uzivatele, ktereho jsme vytvorili
- Prihlasime se 
- **Nemelo** by se nam ukazat "Hi"
- Plocha a data by mely vypadat stejne a po [overeni typu uctu](https://github.com/k4p1cz/spse-zapisy/blob/main/POS/Cviceni/_obecne_utils.md#overeni-typu-uctu-uzivatele)

## Jak omezit velikost sitove slozky Sklad a Home
> Hard quota: nelze prekrocit | Soft quota: lze prekrocit - je zaslano upozorneni administratorovi domeny
### 1. Srv22
- Najdeme si Server manager - otevreme ho a klikneme na **"Add roles and features"**
- Klikneme 2x next, nez se dostaneme k **"Server roles"**
- Vybereme **"File and Storage Services/File and iSCSI Services/File Server Resource Manager"**
- Nainstalujeme
- Prejdeme do **"Tools/File Server Resource Manager"**
- Prejdeme do **"Quotas"** - muzeme vytvorit quotu
> Poznamka: zapis bude dokoncen priste