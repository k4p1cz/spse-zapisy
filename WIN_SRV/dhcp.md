# DHCP
## Jak nastavit DHCP
- Nejprve zaradime stroje do domeny manualne, az pak resime DHCP
- Nainstalujeme si na Srv22 novou featurku - **DHCP Server**
- Nezapomenme v Server Manageru nam bude svitit zluty trojuhelnik - je treba zvolit Administratora, jakozto uzivatele, ktery muze rozdavat IP adresy
- Prejdeme do Tools -> DHCP
- Klikneme na nasi domenu a v action menu dame "Authorize"
- Prejdeme do zalozky IPv4 a v action menu vytvorime novy scope
- Nastavime limity IP adres
- Vse je velmi intuitivni