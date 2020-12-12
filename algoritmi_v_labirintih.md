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

Zgoraj smo si ogledali preprost primer labirinta brez krožnih poti. Ker labirint ni imel krožnih poti, je bil pripadajoči graf brez ciklov. Zato si za boljše razumevaje predstavitve labirinta z grafom oglejmo še primer labirinta s krožnimi potmi.


## Labirint s krožnimi potmi

Dan je labirint:

<img src="./dodatne_slike/lab_2.jpg" width="300" >

Ponovimo že znani postopek številčenja hodnikov.

<img src="./dodatne_slike/lab_2_ostevilcen.jpg" width="300" >

Ugotovimo, da je en hodnik lahko označen z več številkami. To se zgodi samo pri nekaterih hodnikih, ki so del krožnih poti labirinta, kar pa na samo predstavitev labirinta z grafom ne vpliva. Če bi želeli poiskti najdaljšo oz. najkrajšo pot v labirintu s pomočjo grafa, bi preprosto upoštevali dolžine "hodnikov". Opazimo, da skupna vsota dolžin "hodnikov" znotraj enega hodnika v labirintu še vedno ustreza dejanski dolžini tega hodnika. Na primer hodnik označen s `6` in `15` je dolg `5` enot, pri čemer opazimo, da "hodnik" označen s `6` ustreza `1` enoti, "hodnik" označen s `15` pa `4` enotam. Torej je skupna vsota še res enaka: `1 + 4 = 5`.

Dan labirint predstavimo z grafom:

<img src="./dodatne_slike/lab_2_graf.jpg" width="300" >

Spomnimo se, da iščemo pot od vozlišča `1` do vozlišča `9`. Opazimo, da imamo več možnih poti. To se zgodi, ker je graf cikličen. Sedaj označimo eno izmed ustreznih poti.

<img src="./dodatne_slike/lab_2_graf_pot.jpg" width="300" >




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

Za boljšo predstavo, si oglejmo ta algoritem na grafu za naš labirint.

**Primer:** Iskanje v globino začne preiskovanje v poljubnem (izbranem) vozlišču. Pri preiskovanju celotnega grafa se drži pravil:
1. v vozlišču z več povezavami (kjer ima možnost izbire poti) se loti preiskovanja *najbolj desne* nepregledne veje (tj. najbolj levega sina);
   
   <img src="./slike/izbira1.png" width="400" >

2. ko pride v vozlišče z vsemi pregledanimi povezavami, se vrača nazaj po preiskani poti do prvega vozlišča z nepregledano povezavo;
   
   <img src="./slike/izbira2.png" width="400" >

3. ko pregleda celoten graf, se ustavi.

*Opomba:* Če želimo v grafu najti nek točno določen element, sledimo zgornjemu postopku, le da se algoritem ustavi, ko najde iskani element oziroma preišče celoten graf (če taga elementa ni v grafu).

Poglejmo si enega izmed možnih pregledov grafa z uporabo iskanja v globino (barvne številke):

<img src="./slike/pricakovan_pregled.png" width="400" >


### Katero podatkovno strukturo moramo uporabiti za iskanje v globino?

Na primeru si oglejmo delovanje algoritma:

<img src="./slike/pregled_1.png" width="300" >
<img src="./slike/pregled_2.png" width="300" >
<img src="./slike/pregled_3.png" width="300" >
<img src="./slike/pregled_4.png" width="300" >
<img src="./slike/pregled_5.png" width="300" >
<img src="./slike/pregled_6.png" width="300" >
<img src="./slike/pregled_7.png" width="300" >
<img src="./slike/pregled_8.png" width="300" >

Opazino, da mora podatkovna stuktura za algoritem iskanja v globino delovati na principu `zadnji noter, prvi ven`. Torej lahko uporabimo `sklad`.

### Časovna zahtevnost
Ker vemo, da bomo vsako vozlišče pregledali enkrat, prav tako tudi vsako povezavo, je pričakovana časovna zahtevnost iskanja v globino `O(V + P)`, pri čemer je `V = število vozlišč grafa` in `P = število povezav grafa`, če graf predstavimo s tabelo sosednosti.

*Iskanje v globino uporabimo predvsem pri iskanju najkrajše poti.*

## Iskanje v širino

Samo ime nam pove, da ta algoritem deluje na principu pregledovanja v širino. Torej `preiskuje po nivojih` - ko pregleda vsa vozlišča na neki globini, nadaljuje s pregledovanjem vozlišč na naslednji globini.

Za boljšo predstavo si algoritem oglejmo na našem grafu za labirint.

**Primer:** Iskanje v globino začne preiskovanje v poljubnem (izbranem) vozlišču. Pri preiskovanju celotnega grafa se drži pravil:
1. najprej se premika horizontalno in pregleda vsa vozlišča na trenutnem nivoju ter shrani njihove naslednike;
   
   <img src="./slike/bfs1.png" width="400" >

2. premakne se na naslednji nivo;
   
   <img src="./slike/bfs2.png" width="400" >

3. ko pregleda celoten graf, se ustavi.

*Opomba:* Če želimo v grafu najti nek točno določen element, sledimo zgornjemu postopku, le da se algoritem ustavi, ko najde iskani element oziroma preišče celoten graf (če taga elementa ni v grafu). Na ta način v danem grafu najdemo najkrajšo možno pot med korenom in iskanim vozliščem, dolžina pa predstavlja globino, na kateri se nahaja iskano vozlišče.

Poglejmo si enega izmed možnih pregledov grafa z uporabo iskanja v globino (barvne številke):

<img src="./slike/pricakovan_pregled_sirina.png" width="400" >


### Katero podatkovno strukturo moramo uporabiti za iskanje v širino?

Na primeru si oglejmo delovanje algoritma:

<img src="./slike/pregled_1.png" width="300" >
<img src="./slike/pregled_2.png" width="300" >
<img src="./slike/pregled_3_bfs.png" width="300" >
<img src="./slike/pregled_4_bfs.png" width="300" >
<img src="./slike/pregled_5_bfs.png" width="300" >
<img src="./slike/pregled_6_bfs.png" width="300" >
<img src="./slike/pregled_7_bfs.png" width="300" >
<img src="./slike/pregled_8_bfs.png" width="300" >

Opazimo, da mora podatkovna stuktura za algoritem iskanja v širino delovati na principu `prvi noter, prvi ven`. Torej lahko uporabimo `vrsto`.

### Časovna zahtevnost
 Ker vemo, da bomo vsako vozlišče pregledali enkrat, prav tako tudi vsako povezavo, je pričakovana časovna zahtevnost iskanja v globino `O(V + P)`, pri čemer je `V = število vozlišč grafa` in `P = število povezav grafa`, če graf predstavimo s tabelo sosednosti.

*Iskanje v širino uporabimo predvsem pri iskanju najdaljše poti.*

---

# Iskanje najdaljše poti v grafu


Do sedaj smo si ogledali, kako naredimo pregled grafa z uporabo iskanja v globino in širino. Pri tem je bil naš namen preiskovanja najti pot preiskovanja. Kaj pa, če bi želeli iz grafa izvedeti kaj drugega - na primer najdaljšo pot v grafu? Najprej premislimo, katerega izmed algoritmov bomo izbrali. Pri preiskovanju v širino preiskujemo po nivolih. Torej je ta algoritem primernejši za iskanje najkrajše poti. Pri preiskovanju v globino pa na vsakem koraku preiskujemo maksimalno globoko. Torej lahko dobimo dolžine posameznih podgrafov. Poglejmo si na primeru cikličnega grafa. Pri tem se lotimo preiskoovanja najprej z leve, nato pa še z desne.

<img src="./slike/cikel_dol_1.png" width="200" >
<img src="./slike/cikel_dol_2.png" width="200" >

Opazimo, da je najdaljša dobljena dolžina odvisna od smeri pregledovanja. Zato se bomo raje ostredotočili zgolj na aciklične usmerjene grafe. Tako bo smer pregledovanja z usmerjenostjo povezav enolično določena, poleg tega pa bomo z acikličnostjo onemogočili neskončne zanke, ki bi nam pokvarile rezultat - s cikli lahko dobimo poljubno veliko dolžino.


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