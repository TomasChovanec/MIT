# Definice vlastní funkce v jazyce C

Funkce v jazyce C slouží k rozdělení programu na menší, přehlednější části, které lze opakovaně používat.

Hlavní výhody funkcí:
- **Zjednodušují kód** – program je přehlednější a lépe organizovaný.
- **Umožňují opakované použití kódu** – stejnou funkci lze volat vícekrát, čímž se snižuje duplicita.
- **Usnadňují ladění a údržbu** – chyby se hledají snadněji, protože jsou izolovány v konkrétní funkci.
- **Podporují modulární programování** – umožňují rozdělit program na logické části.

Funkce mají název, mohou (ale nemusí) přijímat vstupní parametry a mohou vracet hodnotu.

V prostředí **Microchip Studio** vytváříme program v jazyce C, který následně přeložíme pro mikrokontrolér ATmega2560. Program začíná vykonávat funkci `main()`. Na rozdíl od prostředí Arduino zde používáme vlastní funkci `main()` a program si sami strukturujeme.

### Funkce bez parametrů a návratové hodnoty

Vytvoříme jednoduchou funkci, která rozsvítí LED připojenou k pinu PF0 výukového kitu.

Klíčové slovo `void` před názvem funkce označuje, že funkce nevrací žádnou hodnotu. `void` v závorkách znamená, že funkce nepřijímá žádné vstupní parametry.

```c
#include <avr/io.h>
#include <util/delay.h>

#define F_CPU 16000000UL

void rozsvit_led(void)
{
    PORTF |= (1 << PF0);
}

int main(void)
{
    // Nastavení PF0 jako výstupu
    DDRF |= (1 << PF0);

    while (1)
    {
        rozsvit_led();
        _delay_ms(1000);
    }
}
```

Funkci `rozsvit_led()` jsme definovali mimo funkci `main()`. V hlavním programu ji následně zavoláme příkazem `rozsvit_led();`.

Všimněte si, že funkce může pracovat přímo s registry mikrokontroléru, například `PORTF`. Nemusí tedy vždy přijímat parametry.

### Funkce s parametrem

Funkce může mít libovolný počet vstupních parametrů, které se zapisují do závorek při její definici. Parametry umožňují funkci přizpůsobit její chování podle konkrétních hodnot.

Upravme předchozí příklad tak, aby funkce umožňovala rozsvítit LED na libovolném bitu portu F.

```c
#include <avr/io.h>
#include <util/delay.h>

#define F_CPU 16000000UL

void nastav_led(uint8_t led)
{
    PORTF |= (1 << led);
}

int main(void)
{
    // Nastavení celého portu F jako výstupu
    DDRF = 0xFF;

    while (1)
    {
        nastav_led(0);  // Rozsvítí LED na PF0
        _delay_ms(500);

        nastav_led(3);  // Rozsvítí LED na PF3
        _delay_ms(500);
    }
}
```

Parametr `led` je typu `uint8_t`, tedy nezáporné osmibitové číslo. Při volání funkce předáváme konkrétní hodnotu, která určuje číslo bitu portu F.

### Návratová hodnota funkce

Funkce může také vracet hodnotu. Vytvořme například funkci, která spočítá obsah obdélníku. Výsledek nechceme pouze vypsat, ale chceme jej použít v dalších výpočtech.

Funkce vrací hodnotu pomocí příkazu `return`, který zároveň ukončí její vykonávání. Při definici funkce musíme uvést datový typ návratové hodnoty.

```c
#include <avr/io.h>

int obsah_obdelniku(int sirka, int vyska)
{
    int vysledek = sirka * vyska;
    return vysledek;
}

int main(void)
{
    int sirka = 5;
    int vyska = 10;

    int obsah = obsah_obdelniku(sirka, vyska);

    while (1)
    {
        // Výsledek můžeme dále používat v programu
    }
}
```

Funkce `obsah_obdelniku()` přijímá dva parametry typu `int` a vrací jejich součin. Návratovou hodnotu ukládáme do proměnné `obsah`.

## Úkoly

1. **Blikání LED:** Vytvořte funkci `void blikej(uint8_t pocet_bliknuti)`, která bude blikat LED připojenou k PF0. Například volání `blikej(5);` způsobí pět bliknutí LED. Periodu blikání si zvolte sami. Použijte funkci `_delay_ms()`.

2. **Ovládání LED pomocí parametru:** Vytvořte funkci `void nastav_led(uint8_t cislo_led, uint8_t stav)`, která umožní rozsvítit nebo zhasnout zvolenou LED na portu F. Parametr `stav` bude nabývat hodnot 0 nebo 1.

3. **Měření vzdálenosti:** Pro snadnější práci s ultrazvukovým senzorem vytvořte funkci `uint16_t zmer_vzdalenost(void)`, která změří vzdálenost a vrátí ji v centimetrech. Funkce bude využívat vstupní a výstupní piny mikrokontroléru a měření délky impulzu.
