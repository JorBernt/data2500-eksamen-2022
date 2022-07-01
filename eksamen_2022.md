# Eksamen 2022

### 1. 
    A * B + A


### 2. 
    mv /tmp/fil.txt .

### 3.
    Roten av filsystemet

### 4.

```bash
cd
cat file1
```
    NNlkD

### 5.

```bash
find ~ -name file2
```
    zFyIh

### 6.

```bash
sudo su
cd ~
Her finnes en mappe kalt "..." hvor filen ligger
```
    XTyZa

### 7.

```bash
service docker start
docker ps -a
```
    85ce4157775f

### 8.
```bash
docker start 85ce4157775f
curl localhost
```
    iDNB4

### 9.
```bash
docker cp 85ce4157775f:/stegosaurus.jpeg .
steghide extract -sf stegosaurus.jpeg
r27XT
```
    85ce4157775f

### 10.

    Programmet lager og starter to tråder, som utfører den eksterne funksjonen enline() 10000000 ganger hver.

    Denne funksjonen øker den felles variabelen svar med 1. 

    Siden begge trådene startes likt og kan kjøre på hver sin CPU, og det ikke er gjort noe synkronisering, vil det oppstå race-conditions. 

    Når tråd en har lest verdien fra svar-variabelen, kan det oppstå en context switch, hvor tråd 2 leser svar, øker den, også skriver den tilbake til minne. Når det da switches tilbake til tråd 1, så vil den overskrive svar med sin nå utdaterte verdi.

### 11.


    Når programmet kjøres med taskset -c 0, så tvinger vi programmet til å kun bruke en CPU, CPU 0.

    Siden begge trådene må kjøre på samme CPU, vil de ikke kunne kjøres parallelt. Det vil byttes frem og tilbake mellom hvilken tråd som kjøres (og evt. andre prosesser som kjøres). Siden de ikke kjøres parallelt på 2 cpuer, er det mindre sjangse for at det skjer en context switch i et kritisk avsnitt (når variabelen svar leses og økes). Derfor vil det stort sett bli riktig resultat, men av og til så vil det skje en switch i det kritiske avsnittet, og resultatet blir feil. 

### 12.

    Med opsjonen -S blir programmet kompilert til en assembly fil, som viser assembly-instruksene.

    I de følgende tre linjer ligger årsaken på hvorfor det ikke alltid blir korrekt resultat:

    movl svar(%rip), %eax - Henter svar fra minne, og legger det i register

    addl $1, %eax - Øker verdien i registeret med 1

    movl %eax, svar(%rip) - Legger den nye verdien tilbake igjen i minne

    Dette er de tre instruksene som må utføres for å øke svar med 1.

    Siden en context switch kan skje etter hvilken som helst instruks i dette programmet, kan den også skje etter at svar er lest fra minne og økt (de 2 første linjene), men før den skrives (den siste linjen).

    Om da tråd 2 leser, øker og skriver tilbake i mellomtiden, vil da denne variabelen overskrives av tråd 1 når den for kjøre igjen.

### 13.


    Her byttes en.c ut med minimal.s. Minimal gjør det samme som en, øker svar med 1, men istedenfor at det er C høynivåkode, er det gitt ren assemblykode. De tre instruksene fra forrige oppgave, er byttet ut med en instruks, incl svar(%rip). Denne utfører den samme operasjonen, men kun med en instruks. Derfor forsvinner muligheten for at en tråd kan bli stoppet underveis i det kritiske avsnittet, siden den enten må bli stoppet før denne instruksen, eller etter. Derfor blir resultatet alltid korrekt.

### 14.

    Uten taskset, så vil trådene kunne kjøre på hver sin CPU. Selv om det nå bare er en instruks, så må fortsatt variabelen leses fra minne, økes og skrives tilbake. Trådene vil kjøres så og si helt likt. Dette fører da til at de vil lese samme verdi, øke den med 1, også skrive den tilbake. Siden begge to leste f.eks verdien 10 fra minne, begge øker den til 11, og skriver det tilbake, så vil vi nå ha gått glipp av en verdi. Den har blitt økt to ganger, men verdien er fortsatt bare økt med en.

    Dette kan vi se på resultatet, at sluttresultatet er ca. halvparten av det korrekte resultatet.



    Minimal med lock:

    .globlenlinje
    enlinje:
       lock
       incl svar(%rip)
       ret



    Her kjøres det en hardware lock, som låser minnebussen på CPUen, slik at når en tråd leser og skriver til minne, så kan ingen andre tråder gjøre det samtidig. 

    Derfor vil resultatet her alltid bli korrekt.

### 15.

    Bruker en hardware timer til å gi begrensede tidsintervall til brukerprosessene

### 16.

    90

### 17.

    60

### 18.

    Registere er hurtigere en cache

### 19.

    16 MiB

### 20.

    8204

### 21.

    Begge programmer har en 2 dimensjonal array, og looper over hele og fyller hver plass med verdien 5. 

    Mem1 gjør dette ved å fylle hver rad i rekkefølge. Adressene til hvert element i en rad vil ligge etter hverandre i minne, og derfor kan da hele rader bli hentet i cache samtidig, og dette vil gå veldig å fort å lese og skrive til de, og finne neste adresse.

    I mem2 er det kolonnene som først fylles, og de kan ligge spredt rundt omkring i minne. Da vil vi ikke få noe gevinst med at de neste adressene vil bli hentet samtidig inn i cache, og det må letes etter neste verdi på nytt fra minne for hvert element. Dette tar da mye lenger tid, hele 2 sekunder ekstra.

### 22.

```bash
#! /bin/bash

if [[ -d dir ]]
then
    rm -r dir
fi

mkdir dir

for i in {a..d}
do
    mkdir dir/$i
    for j in {1..4}
    do
        dir=dir/$i/$j
        mkdir $dir
        echo $dir > $dir/fil.txt
    done
done
```

### 23.

```powershell
$fil1=(Get-ChildItem $args[0]).LastWriteTime 
$fil2=(Get-ChildItem $args[1]).LastWriteTime 
$path1=(Get-ChildItem $args[0]).FullName
$path2=(Get-ChildItem $args[1]).FullName
$res=($fil2-$fil1).TotalDays
"$path1 er $res dager eldre enn $path2"
```







