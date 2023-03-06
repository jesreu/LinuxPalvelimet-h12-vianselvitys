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
Ylempänä olevassa kuvassa näkyy, että manage.py on djangon käyttämä komentorivi työkalu, joten oletan, että sitä muokkaamalla ei olisi vaikutuksia sivuston toimintaan. Oletan, että nyt emme voi enää ajaa `./manage.py` komentoja. Kokeillaan ajaa `./manage.py makemigrations`.

    ./manage.py makemigrations
    
![image](https://user-images.githubusercontent.com/112503770/222977101-fc461b44-d61d-4350-b0a7-6543b19f1b9a.png)

NameError näyttäisi kertovan, siitä että nimelle sys ei voida antaa arvoa. 

### Lokimerkinnät
En löytänyt lokimerkintöjä virheestä. Oletan, että virhe aiheutti vain djangon sisäisiä ongelmia niin siitä ei voi saada virheilmoituksia esimerkiksi apachen lokeihin.
### Analysoidaan lokeja
Ei lokimerkintöjä.
### Korjataan ongelma
Käydään korjaamassa manage.py tiedosto

    micro manage.py

![image](https://user-images.githubusercontent.com/112503770/222978178-af89777a-41c7-44c2-b31c-214af60338d2.png)

### Testataan, että oireet ovat kadonneet
Nyt jos ajamme komennon:

    ./manage.py makemigrations

Saamme tulokseksi:

![image](https://user-images.githubusercontent.com/112503770/222978240-fbe3b5a8-d42a-4958-8cc5-7290ba52198b.png)

klo 20:20
## b)
### Aiheutetaan ongelma
Django-projektikansio väärässä paikassa. Siirretään projektikansio eri paikkaan. Siirrän projektikansion jepi kansioon jepico.

    mv jepi jepico

![siirretty rojekti](https://user-images.githubusercontent.com/112503770/223028316-aaa0b5ed-7b7b-4596-9500-d514aa0e53a8.png)

### Oireet
Huomaamme jos yritämme navigoida `http:/localhost/admin/`, että saamme 403-forbidden virhekoodin. 

![image](https://user-images.githubusercontent.com/112503770/223028456-f41e85cd-5a84-44fa-b539-108ee72d7d5a.png)

### Lokimerkinnät
Nyt huomaan, että en saa apachen lokeja auki. 

![image](https://user-images.githubusercontent.com/112503770/223032231-8140df07-c69b-4f29-80c0-7f4b8d264c1e.png)

Kokeilin ajaa komentoa ja muita vaihtoehtoisia `cat, tail, head` useampaan otteeseen, mutta en saanut mitään ulos. 

Etsin verkosta mahdollisia ohjeita 30 - 40 minuuttia, mutta en löytänyt mitään järkevää. Sattumalta tajusin ajaa `sudo systemctl restart apache2` komennon, joka yllättäen korjasi ongelman.

![image](https://user-images.githubusercontent.com/112503770/223034546-dc13ad0e-715f-4d29-ba16-0b54cbf6071d.png)

### Analysoidaan lokeja
Ylempänä olevassa lokikuvassa on aiheutettu tilanne, jossa yritin avata `http:/localhost/`. Lokimerkinnän ensimmäinen kohta on viestin päivämäärä ja kellonaika. Seuraavana on viestin tuottanut moduuli (tässä tapauksessa authz_core) ja viestin vakavuusaste (error). Tämän jälkeen ilmoitetaan prosessin prosessitunnus ja säikeen tunnus. Seuraavaksi on pyynnön ip osoite. Lopuksi on yksityiskohtainen virheilmoitus ja sen virhekoodi(AH00035), joka tässä tapauksessa osoittaa, että palvelimen asetukset estävät yhteyden sijainissa /home/jesser/publicwsgi/jepi.

Toisaalta voidaan päätellä, että loki viittaa .conf tiedoston määritelmiin joissa on eritelty projektin sijainti.

![image](https://user-images.githubusercontent.com/112503770/223038725-71b8543d-787f-4d5a-9636-bcca86880326.png)

Kokeilin ajaa myös configtest, jos se antaisi mitään täydentävää.

![image](https://user-images.githubusercontent.com/112503770/223039258-03816cf5-3012-4915-88d0-8a6fe2d6b68f.png)

KOska virhe ei ole itse apachen puolella niin configtest ei kerro, mitään lisää.
### Korjataan ongelma
Siirretään projekti takaisin oikeaan kansioon.

    mv jepi ..
    
Lokiongelmista oppineena käynnistetään vielä apache uudelleen.

    sudo systemctl restart apache2
    
### Testataan, että oireet ovat kadonneet
Nyt siirtymällä selaimella osoitteeseen `http:/localhost/admin/` huomaamme, että sivu toimii taas.

![image](https://user-images.githubusercontent.com/112503770/223040211-4b03c9b8-4843-400a-bb50-7740669cd482.png)

## c)
### Aiheutetaan ongelma
Projektikansiolla väärät oikeudet. Oikeuksien muuttaminen onnistuu chmod komennolla, mutta koska komento ei ole tuttu ajetaan `man chmod`.

![image](https://user-images.githubusercontent.com/112503770/223041717-8887507b-d097-4a68-b042-0c327c1b8991.png)

![image](https://user-images.githubusercontent.com/112503770/223042087-d3d232e3-14f9-493c-a67e-085e8c89183a.png)

![image](https://user-images.githubusercontent.com/112503770/223041820-165729e0-7b8f-4b78-a2f1-93a0cc0a8d86.png)

![image](https://user-images.githubusercontent.com/112503770/223041882-fab85840-9f99-476d-ba7b-58e6a8fa2c28.png)

![image](https://user-images.githubusercontent.com/112503770/223041956-9a4aab98-504e-4c35-b8aa-eeef172059d5.png)

Kurssin sivuilla on tarjottuna valmiina komennot `chmod ugo-rwx teroco/'` ja `chmod u+rx teroco/`. Man sivujen perusteella ensimmäinen komento poistaa oikeuksia ja toinen lisää niitä.

Ajetaan siis komento `chmod ugo-rwx jepi/` jolla pitäisi poistua oikeudet kansioon.

![image](https://user-images.githubusercontent.com/112503770/223043133-75a7ebe9-eb8b-4014-a42d-f34e9c31ca9a.png)

### Oireet
Huomaamme jos yritämme navigoida `http:/localhost/admin/`, että saamme 403-forbidden virhekoodin. 

![image](https://user-images.githubusercontent.com/112503770/223028456-f41e85cd-5a84-44fa-b539-108ee72d7d5a.png)

### Lokimerkinnät
Katsotaan apachen virhe lokeja. `sudo tail apache2/error.log`

![image](https://user-images.githubusercontent.com/112503770/223043514-9a96b53c-bb7d-4a5d-a9a1-c557d8a46975.png)

Lokimerkinnän ensimmäinen kohta on viestin päivämäärä ja kellonaika. Seuraavana on viestin tuottanut moduuli (tässä tapauksessa core) ja viestin vakavuusaste (error). Tämän jälkeen ilmoitetaan prosessin prosessitunnus (4863) ja säikeen tunnus (1399427935123704). Välissä on virheilmoitus ((13)Permission denied) seuraavaksi on pyynnön ip osoite. Lopuksi on yksityiskohtainen virheilmoitus ja sen virhekoodi(AH01630), joka tässä tapauksessa osoittaa, että jollain tiedostopolun (/home/jesser/publicwsgi/jepi/jepi) osuudella on puuttuvat oikeudet.

Ajoin myös `/sbin/apache2ctl configtest` ja `sudo systemctl apache2 status`, mutta ne eivät antaneet mitään täydentävää.

### Korjataan ongelma
Lisätään oikeudet takaisin komennolla `chmod ugo+rwx jepi/`, kuten man sivuilla kerrottiin, että + lisää ja - poistaa.

![image](https://user-images.githubusercontent.com/112503770/223046749-fe16300a-20fc-4ccf-bb54-6eeb10bc39e3.png)

Käynnistetään myös apache uudelleen. `sudo systemctl restart apache2`
### Testataan, että oireet ovat kadonneet
Nyt siirtymällä selaimella osoitteeseen `http:/localhost/admin/` huomaamme, että sivu toimii taas.

![image](https://user-images.githubusercontent.com/112503770/223040211-4b03c9b8-4843-400a-bb50-7740669cd482.png)

## d)
### Aiheutetaan ongelma
Kirjoitusvirhe Apachen asetustiedostossa. Käydään siis muokkaamassa projektin .conf tiedostoa.

    sudoedit /etc/apache2/sites-available/jepico.conf
    
![image](https://user-images.githubusercontent.com/112503770/223050393-76c2f27a-c3fc-43fc-ae3e-8e120aff9b88.png)

Poistin riviltä 11, directoryn sulkevan > merkin. 
### Oireet
Kävin kokeilemassa, en huomannut mitään ongelmia, joten varmuuden vuoksi ajoin `sudo systemctl restart apache2`. Apache selkeästi lakkasi toimimasta komennon jälkeen.

![image](https://user-images.githubusercontent.com/112503770/223051164-4d7bbd4f-1177-48a5-8316-c72f94d81e83.png)

![image](https://user-images.githubusercontent.com/112503770/223051093-80729988-0d0d-4164-a274-d0b5a681b14a.png)

### Lokimerkinnät
Ajoin ensimmäisenä `sudo systemctl apache2 status`, josta hieman navigoimalla löytyi suoraan tieto, että puuttuvasta > merkistä.

![image](https://user-images.githubusercontent.com/112503770/223051433-d8467d0f-540a-4956-8bd2-6e89208b8b5d.png)

Saman tiedon saa myös ajamalla komennon `/sbin/apache2ctl configtest`

![image](https://user-images.githubusercontent.com/112503770/223051730-f87e232f-7bef-440f-a4f3-6c93b828d2d2.png)

Katsotaan vielä mitä virhe lokeista löytyy.

![image](https://user-images.githubusercontent.com/112503770/223052135-872de3a3-417b-4490-9bd1-205f8a6a876c.png)

Lokimerkinnän ensimmäinen kohta on viestin päivämäärä ja kellonaika. Seuraavana on viestin tuottanut moduuli (tässä tapauksessa mpm_event) ja viestin vakavuusaste (notice). Tämän jälkeen ilmoitetaan prosessin prosessitunnus (5245) ja säikeen tunnus (140193214815552). Lopuksi on yksityiskohtainen virheilmoitus ja sen virhekoodi(AH00491), joka tässä tapauksessa osoittaa, että apache koki jonkin virheen ja lopetti toiminnan. Mikä vastaa oireet osiossa nähtyä toimintaa.

### Korjataan ongelma
Lisätään puutuva > tagi takaisin ja käynnistetään apache uudelleen.

    sudoedit /etc/apache2/sites-available/jepico.conf
    sudo systemctl restart apache2

![image](https://user-images.githubusercontent.com/112503770/223052915-86789af6-6c28-4061-b9e5-34b15438e4c6.png)

### Testataan, että oireet ovat kadonneet
Nyt siirtymällä selaimella osoitteeseen `http:/localhost/admin/` huomaamme, että sivu toimii taas.

![image](https://user-images.githubusercontent.com/112503770/223040211-4b03c9b8-4843-400a-bb50-7740669cd482.png)

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

Käytetty aika: 

## Lähteet:

    https://terokarvinen.com/2023/linux-palvelimet-2023-alkukevat/
    https://terokarvinen.com/2022/deploy-django/
    https://httpd.apache.org/docs/2.4/logs.html
