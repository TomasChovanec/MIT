# Samostatná úloha - Dálkové ovládání LED světla pomocí UARTu

Naprogramujte dva mikrokontroléry ATmega2560, které budou komunikovat přes UART. Jeden bude sloužit jako ovládací jednotka s klávesnicí, druhý bude přijímat příkazy a podle nich nastavovat LEDky.

![image](img/15_UART_2_1.png)

## Požadavky na funkci systému

### Ovládací jednotka

- Čeká na vstup z klávesnice.
- Po stisku klávesy pošle odpovídající příkaz (1 bajt) přes UART
- Příkazy:
    - 'A' – zapnout všechny LED
    - 'B' – zapnout jen liché LED
    - 'C' – vypnout všechny LED


### Jednotka pro ovládání LED
- Přijímá příkazy přes UART
- Podle přijatého znaku nastavuje LEDky
- Pokud přijde neplatný znak, ignoruje ho

### Bonusové úkoly
- Pokud přijde neplatný znak, přijímající jednotka spustí krátký tón bzučáku
- Přidejte možnost zapínat a vypínat LEDky po jedné - klávesa * přidá jednu LEDku, klávesa # zhasne jednu LEDku
- Přidejte možnost LEDky nastavovat otáčením potenciometru 

### [Zpět na obsah](README.md)
