# Nahranie kódu na kontrolér CyberPi/mBot2

## 1. krok - inštalácia jazyku Python

Ako prvý krok si stiahneme Python v najnovšej verzii, v čase písania Python 3.12, podľa návodu pre píslušnú platformu (Linux, Windows, OSX)

## 2. krok - detekcia zariadenia

Ako ďaľší krok pripojíme kábľom s koncovkou USB-C dosku CyberPi/mBot2 k našemu počítaču.

V OS Windows by mal byť vidieť v správcovi zariadení/device manager pod záložkou Porty/Ports zariadenie s označením COMx, kde x bude prirodzené číslo.

V prostrediach Linux a OSX uskutočníme nasledujúci príkaz v príkazovom riadku:
```
$ ls /dev | grep -E '(ttyUSB[[:digit:]]*)|(ttyACM[[:digit:]]*)'
```
Následne by sme mali vidieť zoznam zariadení vo formáte ttyUSBx alebo ttyACMx, kde x je opäť prirodzené číslo.

Z týchto zariadení vyberieme ten, ktorý indetifikuje našu dosku, napr. tak, že ju odpojíme a opäť pripojíme, pričom medzi odpojením a pripojením vykonáme jednu z uvedených metód hore, a ten identifikátor, ktorý zmizne a zasa sa objaví je identifikátor našej dosky.
Tento identifikátor je potrebné si zapamätať, prípadne niekde zapísať.

## 3. krok - inštalácia nástroja ESPtool

Otvoríme si príkazový riadok, v OS Windows je to program CMD, v prostredí OSX a Linux príslučný nainštalovaný príkazový riadok, a zadáme nasledujúci príkaz:
```
pip install esptool setuptools
```
Keď uvedený program skončí, sme pripravený čítať a nahrávať kód.

## 4. krok - základné príkazy nástroja ESPtool

Nástroj ESPtool umožňuje okrem iného čítať a zapisovať skompilovaný kód vo formáte súboru .bin (t. j. čisté binárne dáta)

### Príkaz čítania:
```
esptool.py -p PORT -b BAUD read_flash 0x0 ALL flash_contents.bin
```
Texty PORT a BAUD vymeníme za príslušné údaje, text PORT za hore uvedený port zariadenia podľa príslušného operačného systému, text BAUD za čítaciu rýchlosť (odporúčam hodnotu rýchlosti 921600 pre rýchle čítanie, v prípade problémov rýchlosť 115200, ak to nepomôže, tak rýchlosť 9600, a ak nezaberie ani to, problém je pravdepodobne niekde inde).
Text `read_flash` špecifikuje príkaz čítania pamäte, číslo `0x0` začiatočnú adresu, text `ALL`, s prípadnou výmenou slova `ALL` za číslo, špecifikuje koncovú adresu (pri texte `ALL` sa číta celá pamäť) a text `flash_contents.bin` špecifikuje názov súboru, do ktorého sa zapisuje.

### Príkaz zápisu:
```
esptool.py -p PORT -b BAUD write_flash 0x0 flash_contents.bin
```
Texty PORT a BAUD majú rovnaký význam ako pri príkaze čítania, len sa vzťahujú na príkaz zápisu.
Text `write_flash` špecifikuje príkaz zápisu, číslo `0x0` adresu, od ktorej sa zapisuje a text `flash_contents.bin` názov súboru z ktorého sa dáta čítajú.

## Kompletná dokumentácia nástroja ESPtool

Úplnú dokumentáciu nástroja je možné nájsť na stránke spoločnosti Espressif:
[ESPtool](https://docs.espressif.com/projects/esptool/en/latest/esp32/installation.html)
