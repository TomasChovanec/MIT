# PWM

PWM (pulzně šířkovou modulaci) můžeme využít například k regulaci rychlosti motoru, jasu LEDky nebo k řízení servomotoru. U PWM signálu jsou dvě důležité veličiny - frekvence a střída (duty cycle). Střída nám udává poměr času, kdy je signál v log.1 k času celé periody. Tedy například DC=30% při frekvenci 1kHz znamená, že signál je 0,3ms v  logické jedničce a 0,7ms v logické nule.

<img src="https://github.com/user-attachments/assets/a54b052d-7a31-424e-b982-7bb4a846f13b" width="600"/>

V tomto cvičení budeme využívat Timer1, tedy 16bitový čítač. Tento čítač umí generovat PWM signál až na třech vástupech a to zcela automaticky, bez použití jádra procesoru. Tedy časovač jen na začátku programu nastavíme a generování PWM signálu se děje již bez přispění jádra (tj. nemusíme pro něj psát žádný kód). Pouze pokud budeme chtít parametry PWM signálu změnit, např. nastavit jinou střídu, pak musíme samozřejmě použít příkaz pro změnu nastavení časovače.

V datasheetu vidíme, že Timer1 má 15 různých režimů (s režimy normal a CTC už jsme se setkali v předchozích cvičeníchú). Tentokrát si vybereme si mód 14, tedy Fast PWM, kde můžeme nastavit délku periody registrem ICR1 a střídu jendotlivách signálů pomocí OCR1A, OCR1B a OCR1C.

![image](https://github.com/user-attachments/assets/46d04634-b54f-4302-9edd-85ca25d63233)

Jak funguje PWM v módu 14 (a při nastavení neinvertujícího módu) vidíme na obrázku níže. Časovač Timer1 může generowat PWM signál až na 3 výstupních pinech (OC1A, OC1B, OC1C). Časovač čítá v registru TCNT1 od nuly až do maximální hodnoty, která je uložena v registru ICR1. Při přetečení (nastavení čítače zpět do nuly) nastaví výstupní pin na 1. Při shodě čítače s komparačním registrem OCR1A nastaví pin OC1A zpět do nuly. Stejně je to se zbylámi dvěma piny - shoda s registrem OCR1B vynuluje pin OC1B a stejně tak shoda s registrem OCR1C vynuluje pin OC1C.

![image](https://github.com/user-attachments/assets/a5f092e8-8585-4e9b-a1be-719aa85f66d0)


Přehled hodnot předděličky a odpovídajících period časovač:

![image](https://github.com/user-attachments/assets/01d3932f-f769-4452-97d9-7aab6448f6b2)


Začneme nastavením řídícího registru časovače

![image](https://github.com/user-attachments/assets/84240a48-cf04-4c65-8369-c9a0bf85c769)


Vybereme neinvertující režim:

![image](https://github.com/user-attachments/assets/73045035-97a6-4c02-b3df-2adae6312450)


## Úkoly

1. tododo
2. tpododo