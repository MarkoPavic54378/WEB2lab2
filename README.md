# WEB2lab2

Build aplikacije - npm install;npm install typescript
Start aplikacije - npm start

Obrađene ranjivosti - SQL Injection i Broken Authentification

index.html - stranica
flags.json - zastavice za odabir uključenosti ranjivosti
counter.json - služi prilikom autentifikacije za brojanje pokušaja te provođenje timeouta
init.sql - inicijalizacija baze podataka

U bazi 2 primjera: ('alice', 'alicepass', 4000.00), ('bob', 'bobpass', 2349.49)

1. Toggle na stranici služi za odabir ranjivosti koju želimo testirati
2. sljedeće 2 opcije su hoćemo li uključiti ranjivost ili ne

Kod sql injection:
  prilikom uključene ranjivosti:
    unosite primjere u username!
    primjeri kojima se može testirati:
      ' or 1=1 -- ispisuje se sadržaj cijele tablice (bez salary jer se on u upitu ne traži)
      ' or True -- isto
      ' or True order by 1 -- radi
      ' or True order by 2 -- radi
      ' or True order by 3 -- radi
      ' or True order by 4 -- ne radi
      ' or 1=1 union select null, current_database(), null -- radi i ispisuje informacije o bazi
  inače:
    niti jedan primjer od prije ne radi jer je ranjivost uklonjena s pomoću Whitelistinga

Kod broken auth:
  prilikom uključene ranjivosti:
      primjeri kojima se može testirati:
        ako unesete krivi username (npr. alice1): javlja se da ne postoji
        ako unesete krivu lozinku (npr. username: alice i password: alicepass1): javlja se da je kriva lozinka
        ako unesete username koji u sebi ima znakove osim brojeva i slova (te _) (npr. username: alice'): javlja se krivi format za username
        inače: validni podaci
  inače:
      u svim slučajevima javlja Wrong input
      nakon pet promašenih slučaja, kada korisnik želi ponovno unijeti podatke, pokreće se timeout (5 min)
    
