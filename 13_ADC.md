<script type="text/javascript" id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-svg.js"> </script>

# AD převodník

Mikroprocesor jako digitální součástka dokáže pracovat pouze s digitálním signálem. Stav svých pinů čte pouze jako logickou jedničku nebo logickou nulu. Co když ale chceme měřit analogové hodnoty? Například  výstupní napětí z teplotního čidla, nebo napětí baterie, abychom zjistili stav jejího nabití? K tomu slouží analogově digitální převodník - ADC. 

Většina mikrokontrolerů, včetně toho v našem přípravku má jeden nebo více ADC integrovaný ve svém pouzdře. AD převodník nám převede analogové napětí na vstupním pinu na digitální hodnotu, se kterou pak procesor může dále pracovat.

## Referenční napětí
AD převodník funguje poměrově (ratiometric) to znamená, že hodnota jeho výstupu udává poměr měřeného napětí  a referenčního napětí. 

$$
DigitálníHodnota = RozlišeníADC \times \frac{Vin}{Vref}
$$

Napříkald u 10bitového ADC, kde Vref je 5V a měřené napětí je 2,5V:

$$
1023 \times \frac{2,5}{5} = 512
$$

Zdroj referenčního napětí si můžeme zvolit. Může jim být napájecí napětí mikrokontroleru, vnitřní zdroj referenčního napětí či externě připojený zdroj referenčního napětí (např. speciální obvody, jako je LM4040 nebo LM336).

## Prescaler hodinového signálu
Podobně jako u časovače, musíme nastavit, s jakou frekvencí hodinového signálu bude AD převodník pracovat. Do jisté míry platí, že čím bude převod rychlejší, tím méně bude přesný a naopak. AD převodník použitý v ATmega 2560 vyžaduje frekvenci mezi 50kHz a 200kHz.

## Mutliplexer
Protože AD převodník je v čipu mikrokontroleru jen jeden, ale je užitečné mít možnost měřit analogové napětí na více pinech, lze pomocí multiplexeru připojit AD převodník k různým pinům. Výběr pinu musíme samozřejmě provést dřív, než spustíme měření.


## Důležité registry

![image](https://github.com/user-attachments/assets/3110e411-45c2-4292-869f-4fe0f37e1bc9)

**ADEN** - Nastavení tohoto bitu zapne AD převodník. Pokud převodník nepoužíváme, jeho vypnutím můžeme snížit spotřebu energie.

**ADSC** - Start AD převodu. Zápisem jedničky spustíme převod. Po dokončení převodu se bit automaticky nastaví do nuly. Toho můžeme využít pro detekci toho, zda už je převod dokončen.

**ADPS0:2** - Nastavení předděličky hodinového signálu viz tabulka níže.

![image](https://github.com/user-attachments/assets/9392a6ad-f674-416a-86ed-7e0ec120e727)

**MUX0-5** - Nastavení multiplexeru, tedy výběr pinu, na kterém budeme měřit napětí. Pozor, pátý bit MUX5 je umístěn v registru ADCSRB

**REFS0-1** - Nastavení zdroje referenčního napětí, viz tabulka níže

![image](https://github.com/user-attachments/assets/2547caf1-1521-4e2f-a395-a5adde5ea6c6)

**MUX5** - nejvyšší bit pro nastavení multiplexeru (zbytek bitů je umístěn v registru ADMUX)

![image](https://github.com/user-attachments/assets/fdf72867-e734-4187-98ed-1a396691edee)

![image](https://github.com/user-attachments/assets/5d9e78f5-7b8a-40ef-9b43-b4790e6940f7)

![image](https://github.com/user-attachments/assets/ee141afe-1064-4e5a-af2a-17c9bf33633f)


## Použití ADC

1. Nastavit multiplexer podle toho, na kterém pinu chceme měřit (v našem případě máme potenciometr připojen na pinu PK0 tedy ADC8)
2. Nastavit předděličku hodinového signálu - čím vyšší hodnota, tím pomalejší, ale přesnější převod. V našem případě můžeme klidně použít nejvyšší hodnotu.
3. Nastavit zdroj referenčního napětí - napětí na potenciometru se bude pohybovat od 0V do 5V, proto vybereme jako zdroj referenčního nappětí napájecí napětí Vcc, které je také 5V.
4. Spustit konverzi zápisem jedničky do pinu ADSC.
5. Čekat, dokud se konverze nedokončí. Buď cyklicky vyčítat, zda je bit ADSC už v nule (polling) nebo použít přerušení.
6. Po dokončení konverze je výsledek v registru ADC (16bitový registr).


## Úkoly

1. Měřte pomocí ADC napětí na potenciometru a zobrazujte jej na LCD displeji.
2. Rozsvicujte LEDky podle polohy potenciometru (0V - nesvítí žádná LED, 5V - svítí všechny, 2.5V - svítí 4 LEDky atd).
3. Nastavujte potenciometrem rychlost blikání LEDek (1Hz - 50Hz). Frekvenci zobrazujte na LCD displeji.


### [Zpět na obsah](README.md)

