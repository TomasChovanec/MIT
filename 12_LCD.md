# LCD displej

V této lekci si ukážeme, jak zobrazovat data na LCD displeji, který máme na výukovém přípravku. Konkrétně je to typ displeje HD44780. Abychom si práci s ním zjednodušili, využijeme hotovou knihovnu, kterou do našeho projektu vložíme.

1. Stáhněte si následující dva soubory (přes pravé tlačítko a *Uložit odkaz jako*):
[lcd.c](files/lcd.c) a [lcd.h](files/lcd.h)

2. Přidejte soubory do projektu. Klikněte pravým tlačítkem na název projektu -> *Add -> Existing item*
   
    ![image](img/12_LCD_1.png)

3. Přidejte include knihovny lcd a knihovny stdio do main.c souboru
```c
#include "lcd.h"
#include <stdio.h>
```

## Přidání podpory desetinných čísel

Protože AVR mikrokontrolery mají relativně málo paměti a podpora desetinných čísel je relativně náročná, defaultně překladač do programu podporu desetinných čísel u funkce sprintf() nepřidává. Pokud však chcete desetinná čísla na displeji používat, lze ji povolit (za cenu zvětšení výsledného kódu a tím i zpomalení flashování).

*Project -> Properties -> Toolchain -> Libraries* a zde přidat ```libprintf_flt.a```

![image](img/12_LCD_2.png)

*Project -> Properties -> Toolchain -> AVR/GNU Linker -> General* a zaškrtnout *Use vprintf library(WI,-u,vprintf)*

![image](img/12_LCD_3.png)


## Příklady použití funkcí knihovny lcd.c

```lcd_init(LCD_DISP_ON);```  Inicializace displeje, zapne displej bez kurzoru. Jiné módy jsou např. LCD_DISP_OFF, LCD_DISP_ON_CURSOR, LCD_DISP_ON_CURSOR_BLINK.

```lcd_clrscr();``` Smaže celý displej.

```lcd_gotoxy(0,1);``` Přesune kursor displeje na danou pozici (x- znak, y- řádek)

```lcd_puts("Mam rad MIT!");``` Zobrazí na displeji řetězec znaků

```lcd_command(LCD_MOVE_CURSOR_LEFT);``` Posune text doleva

```lcd_command(LCD_MOVE_CURSOR_RIGHT);``` Posune text doprava

## ASCII kód, funkce sprintf()

Už umíme pomocí fuknce *lcd_puts* zobrazit konstantní textový řetězec. Ale co když chceme zobrazit např. hodnotu číselné proměnné?

Na displeji se data zobrazují pomocí ASCII kódů.

Například textový řetězec ```Ahoj 123``` převedeme do ASCII jako ```0x41 0x68 0x6F 0x6A 0x20 0x31 0x32 0x33```.

Pokud tedy chceme na displeji "vytisknout" hodnotu celočíselné proměnné, musíme každou její číslici převést na ASCII znak. S tím nám pomůže fuknce *sprintf*:

```c
char lcd_buffer[32];
int cislo = 156;
sprintf(lcd_buffer, "Cislo je: %d", cislo);
lcd_puts(lcd_buffer);
```
Vytvoříme si pole lcd_buffer, které musí být dostatečně velké, aby se do něj vešel řetězec, který chceme na displeji zobrazit. Funkce sprintf pak nasi číselnou proměnnou převede na ASCII kód a uloží do pole lcd_buffer. Toto pole pak zobrazíme na displeji pomocí funkce lcd_puts, tak jak jsme to dělali u konstantního textu (např. ```lcd_puts(„ahoj“);```)

To ```%d```je tzv. formátovací parametr, který funkci říká, aby číslo "vytiskla" jako dekadické celé číslo. Další formátovací parametry mohou být třeba ```%X``` pro číslo zapsané v hexadecimální soustavě nebo ```%.2f``` pro desetinné číslo.

![image](img/12_LCD_4.png)

*Zdroj obrázku: https://www.sciencebuddies.org/cdn/references/ascii-table.png*

## Úkoly
**1.** Zobrazte na displeji na jednom řádku své jméno, na druhém příjmení

**2.** Zobrazte na displeji odpočet od 10 do 0 po 500ms. Až dopočítá do 0, začne znovu od 10.
 
**3.** Zobrazte na displeji číslo klávesy stisknuté na klávesnici

**4.** Zobrazte na prvním řádku číslo klávesy stisknuté na klávesnici, na druhém řádku zobrazujte celkový součet všech stisknutých kláves od startu programu.


### [Zpět na obsah](README.md)
