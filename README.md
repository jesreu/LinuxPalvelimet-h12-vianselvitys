# LinuxPalvelimet-h12-vianselvitys
    Kirjoittanut: Jesse Reunamo
    Kurssi:       Linux-palvelimet
    Linkki:       https://terokarvinen.com/2023/linux-palvelimet-2023-alkukevat/
    

Aloitus: klo 19:05. 
Tehtävässä aiheutettaan erinäisiä ongelmia djangon tuotantoasennukseen ja etsimään niistä aiheutuvat oireet. Etsitään ongelmista aiheutuvat lokimerkinnät ja tulkitaan niitä. Korjataan aiheutettu onglema ja tarkistetaan, että oireet ovat hävinneet. Tehtävä on suoritettu LinuxPalvelimet-h11-prod raportissa tehtyyn djangon tuotantoasennukseen.

## a)
### Aiheutetaan ongelma
Kirjoitusvirhe Python-tiedostossa. Käydään muokkaamassa manage.py tiedostoa.

    cd publicwsgi
    source env/bin/activate
    cd jepi
    micro manage.py
    
Poistetaan import sys riviltä 3.

![python vika](https://user-images.githubusercontent.com/112503770/222976762-8b7f839f-6c05-400f-9e23-f9669f366cf3.png)

### Oireet
Ylempänä olevassa kuvassa näkyy, että manage.py on djangon käyttämä komentorivi työkalu, joten oletan, että sitä muokkaamalla ei olisi vaikutuksia sivuston toimintaan. Oletan, että nyt emme voi enää ajaa `./manage.py` komentoja. Kokeillaan ajaa ./manage.py makemigrations.

    ./manage.py makemigrations
    
![image](https://user-images.githubusercontent.com/112503770/222977101-fc461b44-d61d-4350-b0a7-6543b19f1b9a.png)

NameError näyttäisi kertovan, siitä että nimelle sys ei voida antaa arvoa. 

### Lokimerkinnät
-- en löytänyt lokimerkintöjä virheestä. 
### Analysoidaan lokeja
-- ei lokimerkintöjä
### Korjataan ongelma
Käydään korjaamassa manage.py tiedosto

    micro manage.py

![image](https://user-images.githubusercontent.com/112503770/222978178-af89777a-41c7-44c2-b31c-214af60338d2.png)

### Testataan, että oireet ovat kadonneet
Nyt jos ajamme komennon:

    ./manage.py makemigrations

Saamme tulokseksi:

![image](https://user-images.githubusercontent.com/112503770/222978240-fbe3b5a8-d42a-4958-8cc5-7290ba52198b.png)

## b)
### Aiheutetaan ongelma
Django-projektikansio väärässä paikassa
### Oireet

### Lokimerkinnät

### Analysoidaan lokeja

### Korjataan ongelma

### Testataan, että oireet ovat kadonneet

## c)
### Aiheutetaan ongelma
Projektikansiolla väärät oikeudet
### Oireet

### Lokimerkinnät

### Analysoidaan lokeja

### Korjataan ongelma

### Testataan, että oireet ovat kadonneet

## d)
### Aiheutetaan ongelma
Kirjoitusvirhe Apachen asetustiedostossa
### Oireet

### Lokimerkinnät

### Analysoidaan lokeja

### Korjataan ongelma

### Testataan, että oireet ovat kadonneet

## e)
### Aiheutetaan ongelma
Apachen WSGI-moduli puuttuu
### Oireet

### Lokimerkinnät

### Analysoidaan lokeja

### Korjataan ongelma

### Testataan, että oireet ovat kadonneet

## f)
### Aiheutetaan ongelma
Väärät domain-nimet ALLOWED_HOSTS-kohdassa
### Oireet

### Lokimerkinnät

### Analysoidaan lokeja

### Korjataan ongelma

### Testataan, että oireet ovat kadonneet

Lopetus: klo 

## Lähteet:

    https://terokarvinen.com/2023/linux-palvelimet-2023-alkukevat/
    https://terokarvinen.com/2022/deploy-django/
    
