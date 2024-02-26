# WEB SERVER
## Jak nastavit Web Server
- "Web server" - dale jen "WS"
- Na SRV22 si otevreme "add roles and features"
- Pridame si featurku "Web Server" pak 2x next a vybereme "URL Auth" a "Basic auth"
- Na uzivatelskem pocitaci, prihlasen za uzivatele, si otevrene prohlizec a zadame IP adresu SRV22 -> Meli bychom videt defaultni stranku

### OBSAH
1. **[Jak zmenit URL](https://github.com/k4p1cz/spse-zapisy/blob/main/POS/Cviceni/ws.md#jak-zmenit-url)**
2. **[Jak zmenit hlavni stranku](https://github.com/k4p1cz/spse-zapisy/blob/main/POS/Cviceni/ws.md#jak-zmenit-hl-stranku)**
3. **[Jak vytvorit podstranku (www.dom22.local/test)](https://github.com/k4p1cz/spse-zapisy/blob/main/POS/Cviceni/ws.md#jak-vytvorit-dalsi-podstranku-napr-wwwdom22localtest)**
4. **[Jak nastavit povoleni browsovat dalsimi podstrankami (www.dom22.local/info)](https://github.com/k4p1cz/spse-zapisy/blob/main/POS/Cviceni/ws.md#jak-nastavit-povoleni-browsovat-dalsimi-podstrankami-wwwdom22localinfo)**
5. **[Jak nastavit pristup az po zadani hesla (www.dom22.local/news)](https://github.com/k4p1cz/spse-zapisy/blob/main/POS/Cviceni/ws.md#jak-nastavit-pristup-az-po-zadani-hesla)**
6. **[Jak nastavit pristup na sdileny disk S (www.dom22.local/skladka)](https://github.com/k4p1cz/spse-zapisy/blob/main/POS/Cviceni/ws.md#jak-nastavit-pristup-na-sdileny-sklad-s)**




#### Jak zmenit URL
- Prejdeme na SRV22-DC - v toolsech kliknu na "DNS"
- Prejdeme na "forward lookup zone"
- Klikneme pravym do volneho mista a dame "Create alias (CNAME)"
- Do prvniho pole zadam "www" a vyberu stroj, na kterem bezi WS
- Do CMD zadame prikaz na aktualizace DNS zaznamu

#### Jak zmenit hl. stranku
- Prejdeme na SRV22, otevreme pruzkumnika a otevreme adresu "C:/inetpub/wwwroot/"
- Vytvorime soubor index.html a muzeme vytvaret nas web
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NADPIS</title>
    <style type="text/css">
	:root{
             --font: Roboto, sans-serif;
	}
	*{
    	     margin: 0;
    	     padding: 0;
    	     box-sizing: border-box;
	}
	p,h5,h4,h3,h2,h1,a,li,input,textarea{
    	     font-family: var(--font);
	}
	main{
	     width: 100vw;
	     height: 100vh;
	     padding: 1rem;
	}
    </style>
</head>
<body>
    <main>
	<h1>Welcome!</h1>
    <main>
</body>
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.7.1/jquery.min.js"></script>
</html>
```

#### Jak vytvorit dalsi podstranku (napr. www.dom22.local/test)
- Prejdeme na SRV22, otevreme pruzkumnika a otevreme adresu "C:/inetpub/wwwroot/"
- Vytvorime slozku /test a do teto slozky vytvorime soubor "index.html"
- Upravime html soubor a otestujeme

#### Jak nastavit povoleni browsovat dalsimi podstrankami (www.dom22.local/info)
- Prejdeme na SRV22, otevreme pruzkumnika a otevreme adresu "C:/inetpub/wwwroot/"
- Vytvorime slozku /info a do teto slozky muzeme vlozit soubory
- Na SRV22 v server manageru si otevreme tools-> IIS manager
- Prejdu na "SRV22/Sites/Default Web Site/info"
- Otevru "Directory Browsing" (ikonka)
- V pravem menu dam "Enable" - muzu upravovat, co uzivatel muze videt

#### Jak nastavit pristup, az po zadani hesla
- Prejdeme na SRV22, otevreme pruzkumnika a otevreme adresu "C:/inetpub/wwwroot/"
- Vytvorime slozku /news a soubor index.html - do tohoto adresare budou moci vsichni az potom, co zadaji heslo
- Na SRV22 v server manageru si otevreme tools-> IIS manager
- Prejdu na "SRV22/Sites/Default Web Site/news"
- Otevru "Authentication" (ikonka)
- "Anonymous auth" zmenim na disabled a "basic auth" zmenim na enabled

#### Jak nastavit pristup pouze pro konkretniho uzivatele (www.dom22.local/vedeni)
- Prejdeme na SRV22, otevreme pruzkumnika a otevreme adresu "C:/inetpub/wwwroot/"
- Vytvorime slozku /vedeni a soubor index.html - do tohoto adresare bude moci pristoupit pouze uzivetel "v1"
- Na SRV22 v server manageru si otevreme tools-> IIS manager
- Prejdu na "SRV22/Sites/Default Web Site/vedeni"
- Otevru "Authentication" (ikonka)
- "Anonymous auth" zmenim na disabled a "basic auth" zmenim na enabled
- Otevru "Authorization rules" (ikonka)
- Odstranime defaultni rule
- Pridame dalsi rule "Add Allow Rule.." (prava zalozka "actions")
- Vybereme "Specified users" a zadame do pole nazev uzivatele ("v1")
- Otevru "Directory Browsing" (ikonka)
- V pravem menu dam "Enable" - muzu upravovat, co uzivatel muze videt

#### Jak nastavit pristup na sdileny sklad (S:/)
- [Navod na sklad](https://github.com/k4p1cz/spse-zapisy/blob/main/POS/Cviceni/_obecne_utils.md#jak-nastavit-spolecny-adresar-sklad)
- - Na SRV22 v server manageru si otevreme tools-> IIS manager
- Prejdu na "SRV22/Sites/Default Web Site/vedeni"
- Pravym tlacitkem kliknu na "Default Web Site"
- Kliknu na "Add virtual directory"
- Alias dam napr. "skladka"
- Fyzickou cestu zadam po kliknuti na "..." 
- Ok
- Otevru si nastaveni "skladka" a kliknu na "Directory browsing" -> Enable