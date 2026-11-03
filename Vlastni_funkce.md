# Definice vlastní funkce v jazyce C

Funkce v jazyce C slouží k rozdělení programu na menší, přehlednější části, které lze opakovaně používat.

Hlavní výhody funkcí:
- **Zjednodušují kód** – program je přehlednější a lépe organizovaný.
- **Umožňují opakované použití kódu** – stejnou funkci lze volat vícekrát, čímž se snižuje duplicita.
- **Usnadňují ladění a údržbu** – chyby se hledají snadněji, protože jsou izolovány v konkrétní funkci.
- **Podporují modulární programování** – umožňují rozdělit program na logické části.

Funkce mají název, mohou (ale nemusí) přijímat vstupní parametry a mohou vracet hodnotu.

### Funkce bez parametrů a návratové hodnoty

Vytvoříme jednoduchou funkci, která blikne LED číslo 0 na portu F.

Klíčové slovo `void` před názvem funkce označuje, že funkce nevrací žádnou hodnotu. `void` v závorkách znamená, že funkce nepřijímá žádné vstupní parametry.

```c
#define F_CPU 16000000UL
#include <avr/io.h>
#include <util/delay.h>

void blikni_led_0(void)
{
    PORTF &= ~(1 << 0);  // Rozsvícení LED
    _delay_ms(500);

    PORTF |= (1 << 0);   // Zhasnutí LED
    _delay_ms(500);
}

int main(void)
{
    // Nastavení bitu 0 portu F jako výstupu
    DDRF |= (1 << 0);

    while (1)
    {
        blikni_led_0();
    }
}
```

Funkci `blikni_led_0()` jsme definovali mimo funkci `main()`. V hlavním programu ji následně zavoláme příkazem `blikni_led_0();`.

Všimněte si, že funkce může pracovat přímo s registry mikrokontroléru, například `PORTF`. Nemusí tedy vždy přijímat parametry.

### Funkce s parametrem

Funkce může mít libovolný počet vstupních parametrů, které se zapisují do závorek při její definici. Parametry umožňují funkci přizpůsobit její chování podle konkrétních hodnot.

Upravme předchozí příklad tak, aby funkce umožňovala rozsvítit LED na libovolném bitu portu F.

```c
#define F_CPU 16000000UL
#include <avr/io.h>

void rozsvit_led(uint8_t cislo_led)
{
    PORTF &= ~(1 << cislo_led);
}

int main(void)
{
    // Nastavení celého portu F jako výstupu
    DDRF = 0xFF;

    while (1)
    {
        rozsvit_led(0);  // Rozsvítí LED číslo 0
        rozsvit_led(3);  // Rozsvítí LED číslo 3
    }
}
```

Parametr `cislo_led` je typu `uint8_t`, tedy nezáporné osmibitové číslo. Při volání funkce předáváme konkrétní hodnotu, která určuje číslo bitu portu F.

### Návratová hodnota funkce

Funkce může také vracet hodnotu. Vytvořme například funkci, která spočítá obsah obdélníku. Výsledek nechceme pouze vypsat, ale chceme jej použít v dalších výpočtech.

Funkce vrací hodnotu pomocí příkazu `return`, který zároveň ukončí její vykonávání. Při definici funkce musíme uvést datový typ návratové hodnoty.

```c
#define F_CPU 16000000UL
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

1. **Blikání LED:** Vytvořte funkci `void blikej(uint8_t pocet_bliknuti)`, která bude blikat LED číslo 0 na portu F. Například volání `blikej(5);` způsobí pět bliknutí LED. Periodu blikání si zvolte sami. Použijte funkci `_delay_ms()`.

2. **Ovládání LED pomocí parametru:** Vytvořte funkci `void rozsvit_led(uint8_t cislo_led)`, která umožní rozsvítit zvolenou LED na portu F. LED se rozsvítí zápisem logické nuly do příslušného bitu portu F.

3. **Měření vzdálenosti:** Pro snadnější práci s ultrazvukovým senzorem vytvořte funkci `uint16_t zmer_vzdalenost(void)`, která změří vzdálenost a vrátí ji v centimetrech. Funkce bude využívat vstupní a výstupní piny mikrokontroléru a měření délky impulzu.
