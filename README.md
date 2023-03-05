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
    
![image](https://user-images.githubusercontent.com/112503770/222977070-2eaab3b7-f1a3-421f-a985-07cefb61fbde.png)


### Lokimerkinnät

### Analysoidaan lokeja

### Korjataan ongelma

### Testataan, että oireet ovat kadonneet

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
    
