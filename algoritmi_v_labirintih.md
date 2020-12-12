# **ALGORITMI V LABIRINTIH**

**Avtorja:** Tamara Pogačar, Matej Čušin

**Datum:** 3. 12. 2020

---

# Vrste labirintov

Obstaja več vrst labirintov. Kot otroci se srečamo predvsem z 2D labirinti na papirju, ki imajo en vhod in en izhod, potrebno pa je najti poljubno pot med njima. Med učenjem zgodovine naletimo na raznorazne "naravne" labirinte, ki so bodisi zidani, bodisi narejeni iz žive meje (npr. Knosos, razni grajski labirinti). Ko se lotevamo reševanja labirintov, nam ta delitev ni v veliko pomoč. Zato labirinte raje ločimo na:
- dvo-dimenzionalne in več-dimenzionalne;
- standardne (oz. idealne), ki so brez zank in zato ekvivalentni drevesom, in labirinte z zankami.

Pomembno je tudi, kakšen pogled imamo na labrint. Pristop k reševanju je običanjno odvisen od našega vedenja o labirintu - ali se nahajamo zunaj labirinta in imamo pogled na celoten labirint (ptičja perspektiva), ali pa smo postavljeni nekam znotraj labirinta ter do podatkov o labirinu dostopamo s preiskovanjem (sprehodom po labirintu in z beleženjem podatkov o njem).

Ne glede na tip labirinta ter naše začetno vedenje o njem lahko `labirint zapišemo v obliki grafa`. To je uporabno predvsem, ker z grafi znamo operirati, s "slikami" pa v splošnem ne. 


---

# Zapis labirinta v obliki grafa

###### Privzeto po: (Shortest path through a maze, 14. 11. 2020)

<img src="./slike/maze1.png" width="200" >

Ob pogledu na labirint je s prostim očesom običajno težko hitro najti pot med dvema točkama v njem. Ker vemo, da labirint sestoji iz hodnikov in križišč, si ga lahko predstavljamo kot neke vrste graf. V tem grafu bodo `vozlišča predstavljala hodnike`, `povezave pa križišča`, saj na vsakem križišču vemo, do katerih hodnikov lahko dostopamo.

Poraja se vprašanje, kako učinkovito pretvoriti labirint v graf. Če poznamo celoten labirint (pogled s ptičje perspektive), si lahko pomagamo s `številčenjem hodnikov`. Torej: začnemo številčiti v nekem oglišču labirinta in cel hodnik označimo z istim številom. Ko pridemo do križišča, vsakega od naslednjih hodnikov označimo z drugim številom. Pri številčenju v nekem križišču vedno uporabimo le števila, ki pred tem še niso bila uporabljena. Postopek nadaljujemo, dokler ne oštevilčimo celotnega labirinta.

<img src="./slike/maze2.png" width="300" >

S tem postopkom dobimo graf, ki nazorno predstavlja dan labrint. Da najdemo pot v labirintu, poiščemo ustrezno pot v grafu. Pri enostavnih labirintih je ta pot očitna in jo lahko hitro preberemo iz grafa, pri bolj zapletenih pa si je potrebno pomagati z ustreznimi algoritmi za reševanje labirintov (npr. za iskanje najkrajše oz. najdaljše poti, ugotavljanje obstoja poti med dvema točkama, iskanje vseh različnih poti ...).

<img src="./slike/maze3.png" width="400" >

S pomočjo zgornjega grafa lahko določimo eno od možnih poti od vhoda v labirint, predstavljenega z vozliščem `1`,  do izhoda, predstavljenega z vozliščem `11`. Rešitev `1 -> 3 -> 7 -> 11` je predstavljena na spodnji sliki.

<img src="./slike/maze4.png" width="400" >

Če ne poznamo celotnega labirinta (npr. igramo strelsko igro ter se nahajamo znotraj nekega večjega objekta z mnogo hodniki), lahko labirint spoznamo s pomočjo preiskovanja. Nekaj algoritmov za preiskovanje labirintov si bomo ogledali v nadaljevanju.

---

Zgoraj smo si ogledali preprost primer labirinta brez krožnih poti. Ker labirint ni imel krožnih poti, je bil pripadajoči graf brez ciklov (torej dervo). Zato si za boljše razumevaje predstavitve labirinta z grafom oglejmo še primer labirinta s krožnimi potmi.


## Labirint s krožnimi potmi

Dan je labirint:

<img src="./dodatne_slike/lab_2.jpg" width="300" >

Ponovimo že znani postopek številčenja hodnikov.

<img src="./dodatne_slike/lab_2_ostevilcen.jpg" width="300" >

Ugotovimo, da je en hodnik lahko označen z več številkami. To se zgodi samo pri nekaterih hodnikih, ki so del krožnih poti labirinta, kar pa na samo predstavitev labirinta z grafom ne vpliva. Če bi želeli poiskti najdaljšo oz. najkrajšo pot v labirintu s pomočjo grafa, bi preprosto upoštevali dolžine "hodnikov". Opazimo, da skupna vsota dolžin "hodnikov" znotraj enega hodnika v labirintu še vedno ustreza dejanski dolžini tega hodnika. Na primer hodnik označen s `6` in `15` je dolg `5` enot, pri čemer opazimo, da "hodnik" označen s `6` ustreza `1` enoti, "hodnik" označen s `15` pa `4` enotam. Torej je skupna vsota še res enaka: `1 + 4 = 5`. Seveda se lahko deljenemu številčenju hodnikov izognemo z uporabo drugačnega algoritma za številčenje, vendar prepustimo ta razmislek `bralcu za vajo`.

Dan labirint predstavimo z grafom:

<img src="./dodatne_slike/lab_2_graf.jpg" width="300" >

Spomnimo se, da iščemo pot od vozlišča `1` do vozlišča `9`. Opazimo, da imamo več možnih poti. To se zgodi, ker je graf cikličen. Sedaj označimo eno izmed ustreznih poti.

<img src="./dodatne_slike/lab_2_graf_pot.jpg" width="300" >

Opazimo, da so nekatere izmed možnih poti:
* `1 -> 3 -> 4 -> 7 -> 9`
* `1 -> 3 -> 4 -> 8 -> 17 -> 12 -> 10 -> 7 -> 9`
* `1 -> 3 -> 4 -> 8 -> 17 -> 12 -> 10 -> 7 -> 4 -> 3 -> 5 -> 16 -> 13 -> 11 -> 10 -> 7 -> 9`
* `1 -> 3 -> 6 -> 15 -> 13 -> 16 -> 5 -> 3 -> 4 -> 8 -> 17 -> 12 -> 10 -> 7 -> 9`
* `1 -> 3 -> 6 -> 15 -> 13 -> 16 -> 5 -> 3 -> 6 -> 15 -> 13 -> 16 -> 5 -> 3 -> 4 -> 7 -> 9`

Spomnimo se, da v cikličnih grafih obstaja možnost, da se nekaj časa sprehajamo po nekem ciklu, kar v našem primeru labirinta občutno podaljša pot med vhodom in izhodom.


Preden se lotimo preiskovalnih algoritmov, si poglejmo, kako lahko graf predstavimo.


---

# Zapis grafa

Obstaja kar nekaj vrst grafov (usmerjeni, neusmerjeni, oteženi ...). S katero vrsto grafa imamo opravka, je odvisno predvsem od tega, kaj želimo z njim narediti. Torej, če želimo poiskati najkrajšo oz. najdaljšo pot, bomo imeli opravka z `oteženimi grafi`. Pri tem bodo oteži na vozliščih predstavljale dolžine hodnikov v labirintu. Če imamo kakšen enosmeren prehod, bomo imeli opravka z `usmerjenimi grafi`, saj bomo le tako lahko nazorno predstavili omejitve gibanja. Pri tem je dobro vedeti, da nam usmerjene povezave v grafih dajo urejene pare vozlišč. Če želimo le preiskati celoten labirint, v katerem so vsi hodniki prehodni v obe smeri, lahko enostavno vzamemo neotežen neusmerjen graf.

Obstaja več metod zapisa grafa, od katerih sta najpogostejša zapisa z:
- `matriko sosednosti`;
- `tabelo sosednosti`.

Natančneje si bomo ogledali tabelo sosednosti.

## Tabela sosednosti

Tabela sosednosti je tabela tabel, kjer vsaka podtabela vsebuje vozlišče in njegove sosede. Na primer imamo vozlišče `A`, katerega sosedi so vozlišča `B`, `C` in `D`. To zapišemo `'A': ['B', 'C', 'D']`. Dve vozlišči sta sosednji, če je prvo povezano z drugim. Če je graf usmerjen in imamo nekje enosmerno povezavo, moramo paziti, kdo je sosed koga.

**Naredimo tabelo sosednosti za zgornji graf:**
```python
# tabela sosednosti
tab = {
    1: [2, 3],
    2: [1, 4, 5],
    3: [1, 6, 7],
    4: [2],
    5: [2],
    6: [3, 8, 9],
    7: [3, 10, 11],
    8: [6],
    9: [6],
    10: [7],
    11: [7]
    }
```

---

# Preiskovanje grafa


Ogledali si bomo dva algoritma za preiskovanje grafa - `pregled v globino` (eng. depth first search) in `pregled v širino` (eng. breadth first search).

Ker je naš prvotni namen preiskati celoten graf in na koncu izpisati `pot` pregledovanja, bomo v obeh primerih beležili pot iskanja v grafu. Poleg tega bomo beležili `obiskana` vozlišča. Tako bomo zagotovili, da bomo vsako vozlišče obiskali natanko enkrat. Pregled bomo začeli v vozlišču, imenovanem `koren`, ki ga algoritem skupaj s tabelo sosednosti `grafa`  dobi kot vhodni podatek.

Če korena ni v grafu ali pa sam koren ne obstaja (`koren == None`), algoritem sproži napako o neustreznem vnosu podatkov. V nasprotnem primeru ta koren dodamo v ustrezno podatkovno strukturo (označimo `PS`). Ta vsebuje vsa do sedaj še neopredeljena vozlišča - na njih smo pri preiskovanju že naleteli (so neposredni nasledniki že pregledanih vozlišč), a še nismo pregledali njihovih naslednikov. Poleg PS beležimo tudi `obiskana` vozlišča. To so vsa vozlišča, na katera smo naleteli med preiskovanjem.

Na vsakem koraku poberemo vozlišče iz PS (hkrati ga nujno tudi odstranimo).
1. Vozlišče dodamo na pot.
2. Če ima to vozlišče naslednike, jih dodamo v PS ter jih označimo za obiskane.
3. Sicer nadaljujemo preiskovanje, tj. poberemo naslednje vozlišče iz PS.
4. Ko je PS prazna, smo pregledali celoten graf. Tedaj vrnemo pot.

Oglejno si **psevdokodo**:
```python
def naš_algoritem(G, koren):  # G je tabela sosednosti za graf
    # preverimo, ali so vhodni podatki ustrezni
    if koren ni v grafu ali koren ne obstaja:
        return napaka
    # vemo, da so vhodni podatki ustrezni
    naj bo `PS` ustrezna podatkovna stuktura
    PS.vstavi(koren)  # pregled vedno pričnemo v korenu
    obiskan.dodaj(koren)
    ustvari pot
    while PS ni prazen:
        # odstrani vozlišče iz PS, da lahko obišče njegove naslednike
        trenutni = PS.poberi()
        # dodamo trenutnega na pot preiskovanja
        pot.dodaj(trenutni)
        # vse neobiskane naslednike od trenutnega vozlišča doda v PS
        for vsi nasledniki xyz od trenutni v grafu G:
            if xyz še ni obiskan:
                PS.vstavi(xvz)
                obiskan.dodaj(xyz)
    return pot
```
###### Privzeto po: (Depth First Search, 20. 11. 2020)

## Iskanje v globino

Kot že ime pove, ta algoritem deluje na principu pregledovanja v globino. Temelji na `vračanju po isti poti` - pregleda vsa vozlišča s premikanjem naprej v globino grafa, če je le to možno, sicer se vrača do prvega vozlišča, kjer se ponovno lahko premakne globje.

Za boljšo predstavo, si oglejmo ta algoritem na grafu za naš preprost labirint brez krožnih poti.

**Primer:** Iskanje v globino začne preiskovanje v poljubnem (izbranem) vozlišču. Pri preiskovanju celotnega grafa se drži pravil:
1. v vozlišču z več povezavami (kjer ima možnost izbire poti) se loti preiskovanja *najbolj desne* nepregledne veje (tj. najbolj levega sina);
   
   <img src="./slike/izbira1.png" width="300" >

2. ko pride v vozlišče z vsemi pregledanimi povezavami, se vrača nazaj po preiskani poti do prvega vozlišča z nepregledano povezavo;
   
   <img src="./slike/izbira2.png" width="300" >

3. ko pregleda celoten graf, se ustavi.

*Opomba:* Če želimo v grafu najti nek točno določen element, sledimo zgornjemu postopku, le da se algoritem ustavi, ko najde iskani element oziroma preišče celoten graf (če taga elementa ni v grafu).

Poglejmo si enega izmed možnih pregledov grafa z uporabo iskanja v globino (barvne številke):

<img src="./slike/pricakovan_pregled.png" width="300" >


### Katero podatkovno strukturo moramo uporabiti za iskanje v globino?

Na enostavnem primeru si oglejmo delovanje algoritma:

1. Dan imamo graf `G` in koren `1`:
   
   <img src="./slike/pregled_1.png" width="300" >
2. Pregled začnemo v korenu. Torej `1` dodamo v `PS` in ga označimo kot obiskanega, zato ga dodamo v `obiskan`:
   
   <img src="./slike/pregled_2.png" width="300" >
3. Vzamemo `1` iz `PS`, ga dodamo na `pot` in pogledamo njegove sosede. Soseda sta `2` in `32`. Ker še nista bila obiskana, torej nista v `obiskan`, ju dodamo v `obiskan` in v `PS`:
   
   <img src="./slike/pregled_3.png" width="300" >
4. Vzamemo zadnje vozlišče iz `PS`, ga dodamo na `pot` in pogledamo njegove sosede. Torej sosedi od `2` so `1`, `4` in `61`. Ker je `1` že v `obiskan`, z njim ne naredimo ničesar. Torej samo vozlišči `4` in `61` dodamo v `obiskan` in v `PS`:
   
   <img src="./slike/pregled_4.png" width="300" >
5. Ponovno vzamemo zadnje vozlišče iz `PS`, ga dodamo na `pot` in pogledamo njegovo sosede. Sesed od `4` je samo že obiskano vozlišče `2`, zato ne naredimo ničesar:
   
   <img src="./slike/pregled_5.png" width="300" >
6. Vzamemo zadnjega iz `PS`, ga dodamo na `pot` in pogledamo njegove sosede. Edini neobiskani sosed vozlišča `61` je `14`, zato le tega dodamo v `obiskan` in v `PS`:
   
   <img src="./slike/pregled_6.png" width="300" >
7. Ponovno vzamemo zadnje vozlišče iz `PS`, ga dodamo na `pot` in pogledamo njegove sosede. Opazimo, da trenutno vozlišče `14` nima neobiskanih sosedov, zato ne naredimo ničesar:
   
   <img src="./slike/pregled_7.png" width="300" >
8.  Vzamemo zadnje vozlišče iz `PS`, ga dodamo na `pot` in pogledamo njegove sosede. Opazimo, da trenutno vozlišče `32` nima neobiskanih sosedov, zato ponovno ne naredimo ničesar:
   
    <img src="./slike/pregled_8.png" width="300" >
9. Opazimo, da je `PS` prazna, kar pomeni da smo pregledali celoten graf. Torej vemo, da je v `pot` iskana pot preiskovanja.

Opazino, da mora podatkovna stuktura za algoritem iskanja v globino delovati na principu `zadnji noter, prvi ven`. Torej lahko uporabimo `sklad`.

### Časovna zahtevnost
Ker vemo, da bomo vsako vozlišče pregledali enkrat, prav tako tudi vsako povezavo, je pričakovana časovna zahtevnost iskanja v globino `O(V + P)`, pri čemer je `V = število vozlišč grafa` in `P = število povezav grafa`, če graf predstavimo s tabelo sosednosti.

*Iskanje v globino uporabimo predvsem pri iskanju najkrajše poti.*

## Iskanje v širino

Samo ime nam pove, da ta algoritem deluje na principu pregledovanja v širino. Torej `preiskuje po globinag` - ko pregleda vsa vozlišča na neki globini, nadaljuje s pregledovanjem vozlišč na naslednji globini.

Za boljšo predstavo si algoritem oglejmo na našem grafu za labirint.

**Primer:** Iskanje v globino začne preiskovanje v poljubnem (izbranem) vozlišču. Pri preiskovanju celotnega grafa se drži pravil:
1. najprej se premika horizontalno in pregleda vsa vozlišča na trenutni globini ter shrani njihove naslednike;
   
   <img src="./dodatne_slike/pregled_1.jpg" width="300" >

2. premakne se na naslednjo globino;
   
   <img src="./dodatne_slike/pregled_2.jpg" width="300" >
3. ko pregleda celoten graf, se ustavi.
   
   <img src="./dodatne_slike/pregled_7.jpg" width="300" >

*Opomba:* Če želimo v grafu najti nek točno določen element, sledimo zgornjemu postopku, le da se algoritem ustavi, ko najde iskani element oziroma preišče celoten graf (če taga elementa ni v grafu). Na ta način v danem grafu najdemo najkrajšo možno pot med korenom in iskanim vozliščem, dolžina pa predstavlja globino, na kateri se nahaja iskano vozlišče.

Poglejmo si enega izmed možnih pregledov grafa z uporabo iskanja v globino (barvne številke):

<img src="./dodatne_slike/pregled_st.jpg" width="300" >


### Katero podatkovno strukturo moramo uporabiti za iskanje v širino?

Na enostavnem primeru si oglejmo delovanje algoritma:

1. Dan imamo graf `G` in koren `1`:
   
   <img src="./slike/pregled_1.png" width="300" >
2. Pregled začnemo v korenu. Torej `1` dodamo v `PS` in ga označimo kot obiskanega, zato ga dodamo v `obiskan`:
   
   <img src="./slike/pregled_2.png" width="300" >
3. Vzamemo `1` iz `PS`, ga dodamo na `pot` in pogledamo njegove sosede. Soseda sta `2` in `32`. Ker še nista bila obiskana, torej nista v `obiskan`, ju dodamo v `obiskan` in v `PS`:
   
   <img src="./slike/pregled_3_bfs.png" width="300" >
4. Vzamemo prvo vozlišče iz `PS`, ga dodamo na `pot` in pogledamo njegove sosede. Torej soseda od `32` sta `1` in `14`. Ker je `1` že v `obiskan`, z njim ne naredimo nič. Torej samo vozlišče `14` dodamo v `obiskan` in v `PS`.
   
   <img src="./slike/pregled_4_bfs.png" width="300" >
5. Vzamemo prvo vozlišče iz `PS`, ga dodamo na `pot` in ponovimo postopek iz `(2.)`. Torej sosedi od `2` so `1`, `4` in `61`. Ker je bilo vozlišče `1` že obiskani, z njim ne naredimo ničeran, torej dodamo v `obiskan` in v `PS` samo vozlišči `4` in `61`:
   
   <img src="./slike/pregled_5_bfs.png" width="300" >
6. Ponovno vzamemo prvega iz `PS`, ga dodamo na `pot` in ponovimo postopek. Sodeda od `14` sta `61` in `32`, ker pa sta že oba v `obiskan`, ne naredimo ničesar:
   
   <img src="./slike/pregled_6_bfs.png" width="300" >
7. Ponovno vzamemo prvega iz `PS`, ga dodamo na `pot` in pogledamo njegove sosede. Opazimo, da sta bila oba soseda od `61` že obiskana, zato ne naredimo ničesar:
   
   <img src="./slike/pregled_7_bfs.png" width="300" >
8.  Vzamemo prvega iz `PS`, ga dodamo na `pot` in ugotovimo, da je bil njegov sosed (`2`) že obiskan, zato ne naredimo ničesar:
   
      <img src="./slike/pregled_8_bfs.png" width="300" >
9.  Opazimo, da je `PS` prazna, torej vemo, da smo pregledali že celoten graf. Torej je v `pot` iskana pot preiskovanja.

Opazimo, da mora podatkovna stuktura za algoritem iskanja v širino delovati na principu `prvi noter, prvi ven`. Torej lahko uporabimo `vrsto`.

### Časovna zahtevnost
Ker vemo, da bomo vsako vozlišče pregledali enkrat, prav tako tudi vsako povezavo, je `pričakovana` časovna zahtevnost iskanja v globino `O(V + P)`, pri čemer je `V = število vozlišč grafa` in `P = število povezav grafa`, če graf predstavimo s tabelo sosednosti.
`

## Za kakšne grafe delujeta ta algoritma?

Kot je bilo razvidno iz zgornjih primerov, algoritma delujeta na `cikličnih` in `acikličnih` grafih. Po krajšem premisleku ugotovimo, da delujeta tudi na `usmerjenih` grafih. Opazimo pa, da zgoraj opisana implementacija ni ustrezna za pregledovanje `večdelnih` grafov, saj v takem primeru pregledamo le tisti del, v katerem se nahaja `koren`.

Če bi želeli pregledati celoten večdelni graf, bi morali vedeti, katera vozlišča tvorijo dani graf. Ko bi pri izvaljanju algoritma prišli do "konca", torej bi bila `PS` prazna, bi morali preveriti, ali je `množica obiskanih vozlišč enaka množici vseh vozlišč`. Če bi bili enaki, bi vedeli, da smo pregledali celoten graf, sicer pa bi vzeli neko še neobiskano vozlišče za novi `koren` in ponovili postopek. Tu bi se morali zavedati, da ob "koncu" preverimo, ali vsota na novo obiskanih vozlišč in tisti, obiskanih pred tem, ustrezata množici vseh vozlišč danega grafa.

---

# Aplikacija preiskovalnih algoritmov na konkretnem labirintu

Morda aplikacija obravnavanih algoritmov (predstavljenih na grafih) na labirinte ni povsem intuitivna. Zato si oglejmo nekaj primerov delovanja algoritmov na konkretnih labirintih.

---

## Iskanje v globino

### Labirint brez krožnih poti

1. Dan je labirint:
   
   <img src="./dodatne_slike/g_0.jpg" width="300" >

2. Začnemo pri vhodu v labirint. Na tem mestu s `kvadratkom` označimo potožaj v labirintu, s `pikami` ob stranicah kvadrata pa smeri, v katere lahko s trenutnega položaja potujemo. Pri tem označujemo le smeri različne od tiste, s katere smo prišli v trenutno točko:
  
   <img src="./dodatne_slike/g_1.jpg" width="300" >

3. Označujemo celoten hodnik in ko pridemo do križišča, označimo vse možne hodnike, v katere lahko nadaljujemo preiskovanje. Torej s pikami označimo vse potencialne poti preiskovanja:
  
   <img src="./dodatne_slike/g_2.jpg" width="300" >

4. Ker pregledujemo v globino, izberemo najbolj desno pot, torej v našem primeru nadaljujemo z gibanjem navzdol:
  
   <img src="./dodatne_slike/g_3.jpg" width="300" >

5. Ko pridemo do križišča, ponovimo postopek iz točke `(3.)` in nadaljujemo pot navzol:
  
   <img src="./dodatne_slike/g_4.jpg" width="300" >

6. Ponovno si izberemo najbolj desni hodnik in po njem nadaljujemo preiskovanje:
  
   <img src="./dodatne_slike/g_5.jpg" width="300" >

7. Ko pridemo do konca hodnika, ugotovimo, da smo prišli v `slep hodnik`. Torej se moramo vrniti nazaj po preiskovani poti vse do nekega križišča, ki nam ponovno omogoča izbiro poti gibanja. Pri vračanju si beležimo pot gibanja, saj bo le to indiciralo že preiskano pot (uporabimo `rdeče pike`):
  
   <img src="./dodatne_slike/g_6.jpg" width="300" >

8. Ko pridemo do ustreznega križišča, označimo, s katere smeri smo prišli vanj, saj bo le to indiciralo pregledanost hodnika. Ponovno izberemo najbolj desni hodnik in po njem nadaljujemo pot preiskovanja:
  
   <img src="./dodatne_slike/g_7.jpg" width="300" >

9. Ponovno prispemo v neko križišče, si označimo potencialne smeri gibanja in izberemo najbolj desni hodnik ter po njem nadaljujemo preiskovanje:
  
   <img src="./dodatne_slike/g_8.jpg" width="300" >

10. Ponovimo postopek iz točke `(9.)`:
  
    <img src="./dodatne_slike/g_9.jpg" width="300" >

11. Sedaj opazimo, da smo prispeli do iskanega `izhoda`. Na tem mestu lahko preiskovanje zaključimo, saj smo našli vsaj eno pot od `vhoda` do `izhoda`. Če želimo zapisati to pot, se vračamo po poti po kateri smo prispeli do izhoda. Pri tem na vsakem koraku izberemo smer, ki ni označena s piko. Če take poti ni, izberemo tisto, po kateri smo pri prvotnem preiskovanju prišli na trenutno mesto. Na primer v labirintu spodaj na sredini - vemo, da pot naravnost ne bo ustrezna, saj smo pri predhodnjem preiskovanju ugotovili, da gre za slep hodnik. Torej bomo izbrali smer, po kateri smo prišli v to križišče, torej bomo pot nadaljevali desno. Označimo dobljeno pot:

    <img src="./dodatne_slike/g_pot.jpg" width="300" >

12. Če želimo preiskati celoten labirint, preskočimo točko `(11.)` in nadaljujemo s preiskovanjem. Torej se obnašamo tako, kot da bi prispeli v `slep hodnik`. Torej ponovimo postopek iz točke `(7.)`:
  
    <img src="./dodatne_slike/g_10.jpg" width="300" >

13. Prispeli smo v situacijo, podobno tisti iz točke `(8.)`, zato ponovimo že znan postopek:
  
    <img src="./dodatne_slike/g_11.jpg" width="300" >

14. Ponovno spoznamo, da se nahajamo v `slepem hodniku`, zato ponovimo postopek iz točke `(7.)`:
  
    <img src="./dodatne_slike/g_12.jpg" width="300" >

15. Prispeli smo v križišče, ki ima dva možna izhoda. Vemo, da smo enega izmed hodnikov že pregledali (tj. hodnik naravnost - označen z rdečo piko), torej je naša edina možnost potovanja po hidniku na desni:
  
    <img src="./dodatne_slike/g_13.jpg" width="300" >

16. Sprehodimo se po celotnem hodniku prispemo do križišča, iz katerega nadaljujemo potovanje po najbolj desnem še nepregledanem hodniku - torej pot nadaljujemo naravnost:
  
    <img src="./dodatne_slike/g_14.jpg" width="300" >

17. Ponovno ugotovimo, da smo preiskovali slep hodnik, zato ponovimo postopek iz točke `(7.)`:
  
    <img src="./dodatne_slike/g_15.jpg" width="300" >

18. Ko se vračamo po že preiskani poti, pridemo v križiče, za katerega vemo, da ima samo še en hodnik, po katerem laho nadaljujemo pot. To je edini hodnik, ki še ni označen z rdečo piko. Zato pot nadaljujemo po njem:
  
    <img src="./dodatne_slike/g_16.jpg" width="300" >

19. Ugotovimo, da smo se znašli v situaciji analogni tisti, iz točke `(18.)`. Torej ponovno pot nadaljujemo po edinem še ne preiskanem hodniku:
  
    <img src="./dodatne_slike/g_17.jpg" width="300" >

20. Prispeli smo do križiča, iz katerege pot nadaljujemo po najbolj desnem hodniku:
  
    <img src="./dodatne_slike/g_18.jpg" width="300" >

21. Ponovno smo prispeli v slep hodnih, zato se vrnemo nazaj do prvega križišča:
  
    <img src="./dodatne_slike/g_19.jpg" width="300" >

22. Znajdemo se v situaciji analogni tisti, iz točke `(18.)`:
  
    <img src="./dodatne_slike/g_20.jpg" width="300" >

23. Ponovimo postopek iz točke `(20.)`:
  
    <img src="./dodatne_slike/g_21.jpg" width="300" >

24. Ponovimo postopek iz točke `(21.)`:
  
    <img src="./dodatne_slike/g_22.jpg" width="300" >

25. Ponovimo postopek iz točke `(22.)`:
  
    <img src="./dodatne_slike/g_23.jpg" width="300" >

26. Ugotovimo, da smo se vrnili na začetek labirinta (tj. v točko, v kateri smo začeli preiskovanje) ter iz njega ne vodi noben nepregledan hodnik. Torej vemo, da smo preiskali celoten labirint.


**Ugotovimo:** Pri preiskovanju labirinta brez krožnih poti imamo več možnih situacij:

1. preiskujemo hodnik, dokler ne pridemo do nekega križišča:
   * pot nadaljujemo po najbolj desnem še nepregledanem hodniku (tj. hodnik, ki še ni označen z rdečo piko);
   * ugotovimo, da smo prišli na začetek preiskovanja (tj. vhod v labirint) ter smo že preiskali celoten labirint, zato zaključimo s preiskovanjem; 
2. preiskujemo slep hodnik. Ko pridemo do konca, se po njem vrnemo do prvega križišča, v katerem ozančimo, po katerem hodniko smo prišli vanj (tj. narišemo rdečo piko v smeri trenutnega hodnika), in ponovimo postopek iz točke `(1.)`.



### Labirint s krožnimi potmi

Postopek preiskovanja je analogen postopku za preiskovanje labirintov brez krožnih poti. Edina razlika se pojavi pri preiskuju krožne poti. Zato si oglejmo en primer preiskovanja v globinino na labirintu s krožnimi potmi, pri čemer nas ne bo zanimala pot do izhoda, temveč zgolj celoten pregled labirinta.

1. Dan je labirint:
   
    <img src="./dodatne_slike/g0.jpg" width="300" >

2. Začnemo pri vhodu v labirint. Na tem mestu s `kvadratkom` označimo potožaj v labirintu, s `pikami` ob stranicah kvadrata pa smeri, v katere lahko s trenutnega položaja potujemo. Pri tem označujemo le smeri različne od tiste, s katere smo prišli v trenutno točko:
  
    <img src="./dodatne_slike/g1.jpg" width="300" >

3. V križišču označimo vse možne poti preiskovanja. Preiskovanje nadaljujemo po najbolj desni poti:.
  
    <img src="./dodatne_slike/g2.jpg" width="300" >

4. Sprehodimo se skozi celoten hodnik:
  
    <img src="./dodatne_slike/g3.jpg" width="300" >

5. Iz slepega hodnika se vračamo nazaj do prvega križišča:
  
    <img src="./dodatne_slike/g4.jpg" width="300" >

6. Pot nadaljujemo v skrajno desni še nepregledani hodnik:
  
    <img src="./dodatne_slike/g5.jpg" width="300" >

7. Iz trenutnega križišča pot nadaljujemo v srajno desni nepregledani hodnik, torej se gibljemo naravnost:
  
    <img src="./dodatne_slike/g6.jpg" width="300" >

8. Po slepem hodniku se vrnemo do prvega križišča:
  
    <img src="./dodatne_slike/g7.jpg" width="300" >

9. Pot nadaljujemo po najbolj desnem še nepregledanem hodniku:
  
    <img src="./dodatne_slike/g8.jpg" width="300" >

10. Iz križišča preiskovanje nadaljujemo po skrajno desnem hodniku: 
  
   <img src="./dodatne_slike/g9.jpg" width="300" >

11. Pot nadaljujemo po desnem hodniku:
  
  <img src="./dodatne_slike/g10.jpg" width="300" >

12. Našli smo izhod. Ker naš namen ni bil najti pot od vhoda do izhoda, temveč preiskati celoten labirint, se obnašamo, kot da smo naleteli na slep hodnik. Zato se vrnemo po hodniku vse do prvega križošča:
  
   <img src="./dodatne_slike/g11.jpg" width="300" >

13. Pot nadaljujemo po skrajno desnem hodniku - gibljemo se naravnost:
  
    <img src="./dodatne_slike/g12.jpg" width="300" >

14. Iz slepega hodnika se vrnemo do prvega križišča:
  
    <img src="./dodatne_slike/g13.jpg" width="300" >

15. Pot nadaljujemo po edini možni poti in sicer desno:
  
    <img src="./dodatne_slike/g14.jpg" width="300" >

16. Iz trenutnega križišča se gibljemo po skrajno desnem hodniku:
  
    <img src="./dodatne_slike/g15.jpg" width="300" >

17. Prišli smo do točke, ki sicer še ni označena za pregledano (tj. nima rdeče pike, ki bi kazala v smer proti hodniku, v katerem se nahajamo), vendar ima `črno piko`, ki kaže v našo smer. To pomeni, da smo v labirintu našli krožno pot. Ker želimo pregledati preostanek labirinta, se odločimo, da `črno piko spremenimo v rdečo` ter se obnašamo enako, kot v slepem hodniku. Torej se vrnemo po trenutnem hodniku do prvega križišča:
  
    <img src="./dodatne_slike/g16.jpg" width="300" >

18. Pot nadaljujemo po skrajno desnem hodniku:
  
    <img src="./dodatne_slike/g17.jpg" width="300" >

19. Pot ponovno nadaljujemo po skrajno desnem hodniku:
  
    <img src="./dodatne_slike/g18.jpg" width="300" >

20. Pot nadaljujemo po edinem še nepregledanem hodniku:
  
    <img src="./dodatne_slike/g19.jpg" width="300" >

21. Ugotovimo, da smo se vrnili na začetek labirinta (tj. v točko, v kateri smo začeli preiskovanje) ter iz njega ne vodi noben nepregledan hodnik. Torej vemo, da smo preiskali celoten labirint.


**Ugotovimo:** Pri preiskovanju labirinta s krožnimi potmi imamo več možnih situacij:

1. preiskujemo hodnik, dokler ne pridemo do nekega križišča:
   * pot nadaljujemo po najbolj desnem še nepregledanem hodniku (tj. hodnik, ki še ni označen z rdečo piko);
   * ugotovimo, da smo prišli na začetek preiskovanja (tj. vhod v labirint) ter smo že preiskali celoten labirint, zato zaključimo s preiskovanjem; 
2. preiskujemo slep hodnik. Ko pridemo do konca, se po njem vrnemo do prvega križišča, v katerem ozančimo, po katerem hodniko smo prišli vanj (tj. narišemo rdečo piko v smeri trenutnega hodnika), in ponovimo postopek iz točke `(1.)`.
3. preiskujemo hodnik, dokler ne pridemo do točke, ki sicer še ni označena za pregledano (tj. nima rdeče pike, ki bi kazala v smer proti hodniku, v katerem se nahajamo), vendar ima `črno piko`, ki kaže v našo smer (tj. v labirintu smo našli krožno pot). Ker želimo pregledati preostanek labirinta, `črno piko spremenimo v rdečo` ter se obnašamo enako, kot v točki `(2.)`.

---

## Iskanje v širino

Oglejmo si aplikacijo algoritma za iskanje v širino na danih labirintih.

### Labirint brez krožnih poti

1. Dan je labirint:
   
     <img src="./dodatne_slike/s_0.jpg" width="300" >

2. Preiskovajne začnemo pri vhodu v labirint. Na tem mestu s `kvadratkom` označimo potožaj v labirintu, s `pikami` ob stranicah kvadrata pa smeri, v katere lahko s trenutnega položaja potujemo. Pri tem označujemo le smeri različne od tiste, s katere smo prišli v trenutno točko:
  
    <img src="./dodatne_slike/s_1.jpg" width="300" >

3. Ko pridemo do križišča, označimo vse možne poti iz njega in si jih zapomnimo, saj se bomo kmalu vrnili v to križišče in nadaljevali preiskovanje po preostalih poteh iz njega:
  
    <img src="./dodatne_slike/s_2.jpg" width="300" >

4. Torej na tem koraku nadaljujemo preiskovanje po skrajno desnem hodniku. Ko pridemo do križišča si zapomnimo vse poti, ki vodijo iz njega:
  
    <img src="./dodatne_slike/s_3.jpg" width="300" >

5. Vrnemo se do križišča iz točke `(3.)` in nadaljujemo preiskovanje v najbolj desni še nepregledani hodnik:
  
    <img src="./dodatne_slike/s_4.jpg" width="300" >

6. Ker smo po končanem postopku iz točke `(5.)` ugotovili, da je bil hodnik slep, v križišču iz točke `(3.)` pa ni bilo nobenega nepregledanega hodnika več, se premaknemo v križišče iz točke `(4.)` in v njem ponovimo postopek iz točke `(3.)`. Torej pot nadaljujemo v dnajbolj desni hodnik. Ko pridemo do križišča, si zapomnimo vse možne poti iz njega:
  
    <img src="./dodatne_slike/s_5.jpg" width="300" >

7. Vrnemo se nazaj v križišče iz točke `(4.)` in preiščemo preostali hodnik. Ugotovimo, da je hodnik slep:
  
    <img src="./dodatne_slike/s_6.jpg" width="300" >

8. Gremo v križišče iz točke `(6.)` in preiskujemo najbolj desni hodnik. Ugotovimo, da je slep:
  
    <img src="./dodatne_slike/s_7.jpg" width="300" >

9. Vrnemo se nazaj v križišče iz točke `(6.)` in preiskovanje nadaljujemo v najbolj desnem še neprehledanem hodniku. Ko pridemo do križišča, si zapomnimo, do katerih hodnikov lahko iz njega dostopamo:
  
    <img src="./dodatne_slike/s_8.jpg" width="300" >

10. Preiskovanje nadaljujemo v najbolj desni hodnik iz križišča iz točke `(9.)`. Ponovno si zapomnimo možne poti iz končnega križišča preiskovanega hodnika:
  
    <img src="./dodatne_slike/s_9.jpg" width="300" >

11. Vrnemo se nazaj v križišče iz točke `(9.)` in preiščemo preostali hodnik. Zanj ugotovimo, da je slep:
  
    <img src="./dodatne_slike/s_10.jpg" width="300" >

12. Vrnemo se v križišče iz točke `(10.)` in preiščemo najbolj desni hodnik. Ugotovimo, da nas ta hodnik pripelje do izhoda. Ker nas ne zanima pot od vhoda do izhoda, to dejstvo zanemarimo in se obnašamo, kot da smo naleteli na slep hodnik:
  
    <img src="./dodatne_slike/s_11.jpg" width="300" >

13. Vrnemo se v križišče iz točke `(10.)` in preiščemo še zadnji nepregledani hodnik. Ugotovimo, da je tudi ta slep:
  
    <img src="./dodatne_slike/s_12.jpg" width="300" >

14. Ker smo preiskali vse hodnike, ugotovimo, da smo preiskali celoten labirint.


**Ugotovimo:** Pri preiskovanju labirinta brez krožnih poti imamo več možnih situacij:

1. preiskujemo hodnik, dokler ne pridemo do nekega križišča:
   * zapomnimo si vse hodnike, do katerih lahko iz njega dostopamo (nevključno tistega, po katerem smo vanj prišli);
   * ugotovimo, da smo pregledali že vse hodnike, do katerih lahko dostopamo iz trenutnega križišča. Tedaj preiskovanje nadaljujemo iz naslednjega križišča, ki smo si ga predhodnje zapomnili; 
2. preiskujemo slep hodnik. Ko pridemo do konca, vemo, da smo zaključili preiskovanje po tej "veji". Preiskovanje nadaljujemo iz naslednega križišča, ki smo si ga predhodnje zapomnili.


### Labirint s krožnimi potmi

Postopek preiskovanja je analogen postopku za preiskovanje labirintov brez krožnih poti. Edina razlika se pojavi pri preiskuju krožne poti. Zato si oglejmo en primer preiskovanja v širino na labirintu s krožnimi potmi, pri čemer nas ne bo zanimala pot do izhoda, temveč zgolj celoten pregled labirinta.

1. Dan je labirint:
   
    <img src="./dodatne_slike/g0.jpg" width="300" >

2. Začnemo pri vhodu v labirint. Na tem mestu s `kvadratkom` označimo potožaj v labirintu, s `pikami` ob stranicah kvadrata pa smeri, v katere lahko s trenutnega položaja potujemo. Pri tem označujemo le smeri različne od tiste, s katere smo prišli v trenutno točko:
   
    <img src="./dodatne_slike/s1.jpg" width="300" >

3. Pregledamo najbolj desni hodnik in si zapomnimo možne poti iz končnega križišča preiskovanega hodnika:
   
    <img src="./dodatne_slike/s2.jpg" width="300" >

4. Vrnemo se v križišče iz točke `(2.)` in ponovimo postopek iz točke `(3.)`:
   
    <img src="./dodatne_slike/s3.jpg" width="300" >

5. Premaknemo se v križišče iz točke `(3.)` in pregledamo njegov najbolj desni hodnik. Ugotovimo, da je hodnik slep:
   
    <img src="./dodatne_slike/s4.jpg" width="300" >

6. Vrnemo se v križišče iz točke `(3.)` in pregledamo najbolj desni še nepregledani hodnik ter si zapomnimo hodnike, do katerih lahko dostopamo iz končnega križišča:
   
    <img src="./dodatne_slike/s5.jpg" width="300" >

7. Prestavimo se v križišče iz točke `(4.)` in pregledamo najbolj desni še nepregledani hodnik. Ker pregledujemo krožno pot, naletimo na mesto, ki je že bilo pregledano - črna pika kaže v smer hodnika, po katerem smo prišli do tega mesta. V tem primeru vemo, da je sedaj ta hodnik pregledan ter se obnašamo enako kot v primeru slepega hodnika:
   
    <img src="./dodatne_slike/s6.jpg" width="300" >

8.  Vrnemo se v križišče iz točke `(4.)` in poreiskovanje nadaljujemo v zadnji še nepregledani hodnik. Ponovno si zapomnimo, do katerih hodnikov lahko dostopamo iz končnega križišča:
    
    <img src="./dodatne_slike/s7.jpg" width="300" >

9. Prestavimo se v križišče iz točke `(6.)`. Preiščemo še zadnji nepregledani hodnik, za katerega ugotovimo, da je slep:
    
    <img src="./dodatne_slike/s8.jpg" width="300" >

10. Prestavimo se v križišče iz točke `(8.)` in pregledamo njegov skrajno desni hodnik. Ugotovimo, da je slep:
    
    <img src="./dodatne_slike/s9.jpg" width="300" >

11. Vrnemo se v križišče iz točke `(8.)` in pregledamo zadnji še nepregledani hodnik, ki vodi iz njega. Ugotovimo, da je tudi ta hodnik slep:
    
    <img src="./dodatne_slike/s10.jpg" width="300" >

12. Opazimo, da smo pregledali vse hodnike, ki smo si jih tekom preiskovanja zapomnili. Torej vemo, da smo preiskali celoten labirint.

**Ugotovimo:** Pri preiskovanju labirinta s krožnimi potimi imamo več možnih situacij:

1. preiskujemo hodnik, dokler ne pridemo do nekega križišča:
   * zapomnimo si vse hodnike, do katerih lahko iz njega dostopamo (nevključno tistega, po katerem smo vanj prišli);
   * ugotovimo, da smo pregledali že vse hodnike, do katerih lahko dostopamo iz trenutnega križišča. Tedaj preiskovanje nadaljujemo iz naslednjega križišča, ki smo si ga predhodnje zapomnili; 
2. preiskujemo slep hodnik. Ko pridemo do konca, vemo, da smo zaključili preiskovanje po tej "veji". Preiskovanje nadaljujemo iz naslednega križišča, ki smo si ga predhodnje zapomnili;
3. tekom preiskovanja hodnika naletimo na že pregledano mesto - črna pika kaže v smer hodnika, po katerem smo prišli do tega mesta. Torej vemo, da je sedaj ta hodnik pregledan ter se obnašamo enako kot v točki `(2.)`.


---
---

# Iskanje najdaljše poti v grafu


Do sedaj smo si ogledali, kako naredimo pregled grafa z uporabo iskanja v globino in širino. Pri tem je bil naš namen preiskovanja najti pot preiskovanja. Kaj pa, če bi želeli iz grafa izvedeti kaj drugega - na primer najdaljšo pot v grafu? Najprej premislimo, katerega izmed algoritmov bomo izbrali. Pri preiskovanju v širino preiskujemo po nivolih. Torej je ta algoritem primernejši za iskanje najkrajše poti. Pri preiskovanju v globino pa na vsakem koraku preiskujemo maksimalno globoko. Torej lahko dobimo dolžine posameznih podgrafov. Poglejmo si na primeru cikličnega grafa. Pri tem se lotimo preiskoovanja najprej z leve, nato pa še z desne.

<img src="./slike/cikel_dol_1.png" width="200" >
<img src="./slike/cikel_dol_2.png" width="200" >

Opazimo, da je najdaljša dobljena dolžina odvisna od smeri pregledovanja. Zato se bomo raje ostredotočili zgolj na aciklične usmerjene grafe. Tako bo smer pregledovanja z usmerjenostjo povezav enolično določena, poleg tega pa bomo z acikličnostjo onemogočili neskončne zanke, ki bi nam pokvarile rezultat - s cikli lahko dobimo poljubno veliko dolžino.

---

# Iskanje najdaljše poti v acikličnem usmerjenem grafu

V acikličnem usmerjenem grafu želimo poiskati najdaljšo pot. Najdaljša pot v neoteženem grafu je pot z največ vozlišči. Pot je dolga toliko, kolikor povezav vsebuje.

Oglejmo si primer takega grafa:

<img src="./slike/dag.png" width="300" >

Ta graf ima 6 vozlišč in 9 usmerjenih povezav ter je brez ciklov. Ena izmed najdaljših poti za ta graf je `54 -> 4 -> 37 -> 11 -> 1`:

<img src="./slike/dag_najdaljsa.png" width="300" >

Očitno je najdaljša pot res dolga 4.


## Naivni pristop
Spomnimo se algoritmov za iskanje v globino in širino. Ker nas zanima kako `globoko` lahko pridemo iz nekega vozlišča, bomo uporabili algoritem za iskanje v globino.


### Algoritem:
1. Najprej graf razbijemo na podgrafe. To naredimo za vsako vozlišče posebej.
   
   <img src="./slike/pod1.png" width="100" >
   <img src="./slike/pod4.png" width="200" >
   <img src="./slike/pod11.png" width="100" >
   <img src="./slike/pod37.png" width="100" >
   <img src="./slike/pod54.png" width="200" >
   <img src="./slike/pod71.png" width="200" >

2. V vsakem grafu poiščemo vse možne poti in določimo njihove dolžine. To naredimo z iskanjem v globino (V tem primeru nas zanimajo dolžine poti v podgrafih, ne pa pot preiskovanja grafa. Torej se bo dolžina poti shranila vsakič, ko se bo algoritem moral vračati. Ob vračanju moramo pozabiti na vsa vozlišča, preko katerih se vračamo.) V našem primeru dobimo poti z dolžinami:

   <img src="./slike/1.png" width="100" >
   <img src="./slike/4.png" width="200" >
   <img src="./slike/11.png" width="100" >
   <img src="./slike/37.png" width="100" >
   <img src="./slike/54.png" width="300" >
   <img src="./slike/71.png" width="300" >

3. Za vsak podgraf si zapomnimo dolžino najdaljše poti.
   ```
    vozlišče  1: 0
    vozlišče  4: 3
    vozlišče 11: 1
    vozlišče 37: 2
    vozlišče 54: 4
    vozlišče 71: 4
   ```
4. Iskana najdaljša pot danega grafa je enaka najdaljši poti podgrafov.
   ```
    najdaljša pot = 4
   ```

### Časovna zahtevnost
Časovna zahtevnost tega postopka je `O(V^2)`, kjer je `V = število vozlišč`.


## Boljši pristop

*Topološko urejanje*

<img src="./slike/top_prazen.png" width="500" >

Topološko razvrščanje tega grafa je: `1 2 3 4 5`. <br>
Za graf je možno več različnih topoloških urejenosti. Za graf, ki je naveden zgoraj, je topološko razvrščanje: `1 2 3 5 4`<br>
Pri topološkem razvrščanju graf ne sme vsebovati ciklov. Da bi to dokazali, predpostavimo, da obstaja krog iz vozlišč `v_1`, `v_2` ... `v_n`. To pomeni, da obstaja usmerjena pot med `v_i` in `v_i+1` `(1 <= i > n)` in med `v_n` in `v_1`. S topološkim razvrščanjem, moramo priti prej do `v_n` kot do `v_1`. Jasno je, da bo prišel `v_i+1` po `v_i`, ker mora biti zaradi napotkov od `v_i`  do `v_i+1` povezava, to pomeni, da je `v_1` pred `v_n`. No, očitno smo prišli do protislovja. Topološko razvrščanje je torej mogoče doseči samo za usmerjene in aciklične grafe.

Poglejmo, kako lahko v grafu najdemo topološko razvrščanje. Torej v bistvu želimo najti permutacijo oglišč, v kateri velja za vsako oglišče `v_i`  in za vsa vozlišča `v_j`, ki imajo povezavo na `v_i`, da pridejo pred `v_i`. Uporabili bomo tabelo `T`, ki bo označevala naše topološko razvrščanje. Recimo, da imamo za graf z `N` vozlišči seznam `sez` velikosti `N`, katerega *i-ti* element pove število vozlišč, ki še niso vstavljena v `T` in imajo povezavo do `v_i`. Ko bomo `v_i` dodali v `T`, bomo zmanšali vrednost `sez[v_j]` za ena, za vsa vozlišča od `v_i` do `v_j`. Ko smo to naredili, smo dodali povezavo do `v_j`. Tako lahko kadar koli vstavimo samo tista vozlišča, za katera je vrednost `sez[vozlišče] = 0`.
Psevdo koda algoritma:
```python
def topolosko_urejanje(N, matrika_sosednosti):
   '''
      topološko uredi vozlišča, kar za nas pomeni vozlišča od "najbolj osnovnega" lista do tistega, 
      ki je "najvišje v grafu"
      ker pa želimo dobiti najdaljšo pot, je to ravno tisto, kar želimo
   '''
   ustvarimo prazen T
   ustvarimo tabelo sez samih ničel, prav tako tudi tabelo obiskani samih ničel
   potem gremo z zankama preko matrike sosednosti:
      če je v matriki_sosednosti True na tem mestu,
         sez povečamo za ena na tem mestu
   Potem na indexih z vrednostjo 0 v sez
      element postavimo v vrsto in tudi obiskani[element] nastavimo na True
   Sedaj pa delamo, dokler se nam vrsta ne izprazni:
      prvi element vstavimo v T in ga odstranimo,
      spet gremo po zanki vozlišč.
         Vse elemente v matriki_sosednosti[element iz vrha vrste][index] in obiskani[index] ni True
            sez[index] -= 1
            Če je sedaj sez[index] 0,
               to vozlišče vstavimo v vrsto in v tabeli obiskani postane True 
   Vrnemo T
```

Vzemimo si labirint na vrhu tega razdelka. Njegov graf zgleda tako:<br>
<img src="./slike/top1.png" width="500" >

```
Vrsta = 0
sez = 0 1 1 3 2 3
      0 1 2 3 4 5
T = {}
```


```
Vrsta = 1
sez = 0 0 1 2 2 3
      0 1 2 3 4 5
T = 0
```

Torej izbrišemo 0 iz vrste in dodamo v `T`. Vozlišča, ki so neposredno povezana z 0, so 1 in 3, zato jim zmanjšamo vrednost v `sez[i]` za ena. V vrsto pa porinemo 1.

```
Vrsta = 2
sez = 0 0 0 1 2 3
      0 1 2 3 4 5
T = 0 1
```

Nato izbrišemo 2 iz vrste in jo dodamo v `T`. S tem se zmanjša vrednost `sez[i]` pri 4, 5.
```
Vrsta = 3
sez = 0 0 0 0 1 2
      0 1 2 3 4 5
T = 0 1 2
```

Nato izbrišemo 3 iz vrste in jo dodamo v `T`. S tem se zmanjša vrednost `sez[i]` pri 4, 5. sez[4] postane 0, zato 4 vstavimo v vrsto.
```
Vrsta = 4
sez = 0 0 0 0 0 1
      0 1 2 3 4 5
T = 0 1 2 3
```

Nato izbrišemo 4 iz vrste in jo dodamo v `T`. S tem se zmanjša vrednost `sez[i]` pri  5. sez[5] postane 0, zato 5 vstavimo v vrsto.
```
Vrsta = 5
sez = 0 0 0 0 0 0
      0 1 2 3 4 5
T = 0 1 2 3 4
```
Nato izbrišemo 5 iz vrste in dodamo v `T`. S tem je algoritem zaključil - vrne `T`.
```
Vrsta = {}
sez = 0 0 0 0 0 0
      0 1 2 3 4 5
T = 0 1 3 2 4 5
```
*Najdaljša pot v tem labirintu zgleda:*

<img src="./slike/top_res.png" width="500" >


## Boljši pristop (druga možnost)

*Dinamično programiranje*

Oglejmo si primer grafa za vozlišče 11 ter za vozlišče 37.

<img src="./slike/pod11.png" width="100" >
<img src="./slike/pod37.png" width="100" >

Opazimo, da je graf za vozlišče 11 podgraf grafa za vozlišče 37. Če si nato ogledamo še grafa za vozišči 37 in 4, opazimo, da je graf za vozlišče 37 podgraf grafa za vozlišče 4.

<img src="./slike/pod37.png" width="115" >
<img src="./slike/pod4.png" width="200" >

Opazimo, da nekatere dele obravnavamo večkrat. Že pridobljene podatke za podprimere bi želeli uporabiti za nadaljno obravnavo večjih primerov (npr. podatek o najdaljši poti v grafu 37 želimo nadalje porabiti za izračun najdaljše poti za graf 4). Zato nas pri iskanju najdaljše poti v grafu zanima, ali obstajajo kakšni podprimeri, katerih podatke lahko shranimo in kasneje ponovno uporabimo, s čemer bi posledično privarčevali čas.

Najprej se moramo vprašati, kateri je naš najosnovnejši primer. To je vedno celota, ki ima najmanj izhodnih povezav. Ker obravnavamo aciklične usmerjene grafe, vemo, da obstaja vsaj eno vozlišče brez izhodnih povezav. V našem primeru je to vozlišče 1. Ker vemo, da tako vozlišče nima izhodnih povezav, vemo zagotovo, da bo število poti skozi tako vozlišče enako 0.

<img src="./slike/dol1.png" width="70" >

Vsako vozlišče, ki kaže v najosnovnejšo enoto, jo lahko ponovno uporabi. Vrednost tega vozlišča bo `vrednost najosnovnejše enote + razdalja med najosnovnejšo enoto in trenutnim vozliščem`. Ker imamo neotežen graf, bo ta razdalja med poljubnima sosednima vozliščema enaka `1`.

<img src="./slike/dol2.png" width="200" >

*Oglejmo si splošnejši primer:*
###### Pridobljeno s (Longest path in a Directed Acyclic graph | Dynamic Programming | GeeksforGeeks, 20. 11, 2020)

<img src="./slike/dol3.png" width="300" >

Zanima nas najdaljša pot iz vozlišča `a`. Vozlišče `a` lahko dostopa do podatkov za dolžine najdaljših poti skozi njegove sinove `b`, `c` in `e`. Za vozlišče `e` ne vemo nič o njegovih potomcih, vemo le, da je najdaljša pot skozenj enaka `x`. Dolžino najdaljše poti skozi poljubno vozlišče `i` označimo z `dol(i)`. Vemo, da je `dol(b) = 0`, `dol(c) = 1 + dol(d) = 1 + 0 = 1` ter `dol(e) = x`. Ker želimo, da je dolžina skozi vozlišče `a` največja, lahko kar zapišemo `dol(a) = 1 + max(dol(b), dol(c), dol(e)) = 1 + max(0, 1, x)`.


### Časovna zahtevnost
Časovna zahtevnost tega algoritma je `O(V)`, kjer je `V = število vozlišč`, saj vsako vozlišče obiščemo samo enkrat.



---

# Viri:
Depth First Search Algorithm In Python (Multiple Examples) (8. 11. 2020) Pridobljeno s: https://likegeeks.com/depth-first-search-in-python/#:~:text=%20Depth%20First%20Search%20algorithm%20in%20Python%20%28Multiple,to%20implement%20the%20DFS%20algorithm%20in...%20More%20

Shortest path through a maze (14. 11. 2020) Pridobljeno s: https://aakritty.wordpress.com/2014/03/02/shortest-path-through-a-maze/

Maze (10. 11. 2020) Prisobljeno s https://en.wikipedia.org/wiki/Maze

Search A Maze For Any Path - Depth First Search Fundamentals (Similar To "The Maze" on Leetcode) (7. 11. 2020) Pridobljeno s: https://youtu.be/W9F8fDQj7Ok

 Depth First Search (20. 11. 2020) Pridobljeno s: https://www.hackerearth.com/practice/algorithms/graphs/depth-first-search/tutorial/

 Breadth First Search (20. 11. 2020) Pridobljeno s: https://www.hackerearth.com/practice/algorithms/graphs/breadth-first-search/tutorial/

 Longest path in a directed Acyclic graph | Dynamic Programming (20. 11. 2020) Pridobljeno s: https://www.geeksforgeeks.org/longest-path-in-a-directed-acyclic-graph-dynamic-programming/

 Longest path in a Directed Acyclic graph | Dynamic Programming | GeeksforGeeks (20. 11. 2020) Pridobljeno s: https://youtu.be/YxF-x3imVFA