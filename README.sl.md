# Shramba Storžev - Zbirka vzorcev za Jelko FMF

## O projektu

Ta repozitorij vsebuje vse vzorce, ki tečejo na [Jelki FMF](https://jelka.fmf.uni-lj.si/). Vzorci so na voljo v mapi `patterns`. Vsak vzorec je v svoji mapi in je lahko napisan v večini programskih jezikov, ki podpirajo izpis na standardni izhod in se lahko izvajajo v Docker vsebniku. Vsi vzorci iz repozitorija se samodejno prevedejo in namestijo na strežnik Jelkob ter bodo prikazani na uradni novoletni jelki Fakultete za matematiko in fiziko Univerze v Ljubljani.

## Prispevanje

> [!TIP]
> Če ne znate uporabljati Gita/GitHuba ali ne morete slediti tem navodilom iz kakršnega koli drugega razloga, si lahko ogledate [Jelkly](https://jelkly.fmf.uni-lj.si/docs). 
> Nudi orodje za vizualno programiranje, podobno Scratchu, za ustvarjanje in pošiljanje svojih vzorcev za Jelko FMF, brez potrebe po programerskem znanju.

### Splošne informacije

Vse vzorce najdete v mapi `patterns`. Vsak vzorec vključuje svojo mapo, ki mora vsebovati datoteki `config.yml` in `main.*` ter opcijsko `Dockerfile`. Ime mape služi kot ID vzorca. Ime mape naj vsebuje le male črke, številke ter pomišljaje.

Datoteka `config.yml` vsebuje konfiguracijo vzorca. Nastavlja osnovne informacije o vzorcu, kot so ime, opis, avtor in šola. Ime mora biti kratko, opisno in unikatno, medtem ko naj opis v stavku ali dveh razloži, kaj vzorec počne.

Datoteka `main.*` je glavna datoteka vzorca, ki se izvede, ko se vzorec zažene. Če pišete vzorec v jeziku, za katerega nudimo predlogo, bo jezik samodejno zaznan glede na pripono datoteke. Če vzorec pišete v jeziku, za katerega ne nudimo predloge, boste morali napisati tudi lasten `Dockerfile`, ki bo uporabljen za prevajanje in zagon vzorca. Med zagonom naj `Dockerfile` zažene vzorec in preusmeri izhod vzorca na `/tmp/jelka`.

Če pišete vzorec v jeziku, za katerega nudimo predlogo, lahko kljub temu napišete lasten `Dockerfile`, če potrebujete dodatno konfiguracijo, vendar je priporočeno uporabljati privzeto predlogo, če je le mogoče.

### Dodajanje vzorca

Najprej naredite razcep repozitorija in ga klonirajte na svoj računalnik:

```bash
git clone https://github.com/Jelka-FMF/Storzi.git
cd Storzi
```

Nato namestite zahtevane knjižnice:

```bash
pip install -r requirements.txt
```

Za dodajanje novega vzorca ustvarite novo mapo v mapi `patterns` ter dodajte datoteki `config.yml` in `main.*`. Preglejte navodila zgoraj za splošne informacije o datotekah ter spodnje razdelke, specifične za jezik.

Ko ste vzorec dodali (in ga ustrezno oblikovali), shranite spremembe:

```bash
git add patterns/ime-vzorca
git commit -m "Vaše sporočilo ob shranjevanju"
```

Nato lahko svoje spremembe potisnete in ustvarite pull request.

### Python vzorci

Ko pišete Python vzorec, namestite priporočene knjižnice iz datoteke `requirements.txt` v korenu repozitorija:

```bash
pip install -r requirements.txt
```

To bo namestilo vse razpoložljive knjižnice, ki jih lahko uporabljate v Python vzorcih, poleg simulacije za lokalni zagon vzorcev in razvojnih orodij. Vaš vzorec lahko uporablja [Jelka Python API](https://github.com/Jelka-FMF/JelkaPy) in druge razpoložljive knjižnice (glejte spodaj).

Glavno ime datoteke vzorca mora biti `main.py`. Med razvojem vzorca ga lahko zaženete lokalno z uporabo simulacije:

```bash
jelkasim patterns/ime-vzorca/main.py
```

Pred shranjevanjem vzorca se prepričajte, da je ustrezno oblikovan:

```bash
ruff check patterns/ime-vzorca
ruff format patterns/ime-vzorca
```

### JavaScript vzorci

Ko pišete JavaScript vzorec, namestite priporočene knjižnice iz datoteke `package.json` v korenu repozitorija:

```bash
npm install
```

To bo namestilo vse razpoložljive knjižnice, ki jih lahko uporabljate v JavaScript vzorcih, poleg razvojnih orodij. Še vedno pa morate namestiti Python odvisnosti, kot je specifirano v zgornjem razdelku, saj se uporabljajo za zagon simulacije.

Vaš vzorec lahko uporablja [Jelka JavaScript API](https://github.com/Jelka-FMF/JelkaJS) in druge razpoložljive knjižnice (glejte spodaj). Glavno ime datoteke vzorca mora biti `main.js`. Med razvojem vzorca ga lahko zaženete lokalno z uporabo simulacije:

```bash
jelkasim node patterns/ime-vzorca/main.js
```

Pred shranjevanjem vzorca se prepričajte, da je ustrezno oblikovan:

```bash
npm run format patterns/ime-vzorca
```

### Vzorci v drugih jezikih

Za pisanje vzorcev v drugih jezikih je potrebno izdelati `Dockerfile`, ki bo gradil in zagnal vašo kodo. Docker vsebniki pritrdijo cev `/tmp/jelka`, kamor se morajo zapisovati informacije o barvah. To lahko dosežete bodisi z neposrednim pisanjem v cev bodisi z preusmeritvijo standardnega izhoda v cev (kot je prikazano v primerih `python` in `js`).

Trenutni format sledi temu vzorcu:

``` 
#<barva v hexadecimalni obliki><naslednja barva v hexadecimalni obliki> ... <zadnja barva v hexadecimalni obliki>\n
```

Glavne smernice za predstavitev barv:
- Vsaka sličica je na svoji vrstici
- Barve naj bodo zapisane le v hexadecimalni obliki (veljavna bela je `ffffff`, ne pa `#ffffff`)
- Predpona `#` se uporabi samo na začetku vrstice
- Vrstice, ki se ne začnejo z `#`, bodo obravnavane kot komentarji in ignorirane

Za konfiguracijo hitrosti prikazovanja slik (fps) ali drugih informacij pošljite glavo v isto cev `/tmp/jelka` pred zagonom vzorca. Primer glave, ki zadostuje za večino primerov:

``` 
#{"version": 0, "led_count": 500, "fps": 60}
``` 

Pomembne opombe:
- Hitrost prikaza je strojno omejena na 66 sličic na sekundo, zato ne pričakujte več kot 60 fps
- Številni jeziki ne omogočajo avtomatskega praznjenja izhodnega toka, zato boste morda morali implementirati ročno praznjenje

### Smernice
* Vzorci morajo biti napisani tako, da jih je mogoče zagnati v Docker vsebniku.
* Vzorci morajo izhod posredovati na standardni izhod (ki je preusmerjen v `/tmp/jelka`).
* Vzorci ne smejo prikazovati neprimerne vsebine.

## Predloge

### Python
* Osnovna slika: [`images/python`](images/python)
* Privzeta predloga: [`defaults/python`](defaults/python)
* Razpoložljive knjižnice: [`requirements.txt`](images/python/requirements.in)
* Ime datoteke vzorca: `main.py`

### JavaScript
* Osnovna slika: [`images/javascript`](images/javascript)
* Privzeta predloga: [`defaults/javascript`](defaults/javascript)
* Razpoložljive knjižnice: [`package.json`](images/javascript/package.json)
* Ime datoteke vzorca: `main.js`

## Struktura

* `patterns/` - Imenik, ki vsebuje vse vzorce
  * `ime-vzorca/`
    * `config.yml` - Konfiguracijska datoteka za vzorec
    * `main.*` - Glavna datoteka vzorca
* `defaults/` - Privzete Docker predloge za vzorce
  * `javascript/` - Privzeta Docker predloga za JavaScript vzorce
  * `python/` - Privzeta Docker predloga za Python vzorce
* `images/` - Osnovne Docker podobe za vzorce
  * `javascript/` - Osnovna Docker podoba za JavaScript vzorce
  * `python/` - Osnovna Docker podoba za Python vzorce

## Licenca

S prispevanjem v ta repozitorij se strinjate, da licencirate svoje delo pod licenco MIT.
