# Manuál pro použití

## 1. Připojení sond a čerpadel

- Na jednotlivé porty multiplexorů sond a čerpadel připojte prvky. Porty si vzájemně odpovídají, tudíž pokud sonda na portu 14 multiplexoru 1 naměří sucho, multiplexor 2 na portu také 14 pustí čerpadlo.

## 2. Konfigurace

- V kódu firmwaru pro Arduino v záhlaví souboru je potřeba vyplnit pole ACTIVE_PORTS. Zde napiště všechny porty, které budou mět být obsluhovány, a také časový interval, který bude mezi jednotlivými cykly obsluhy.

- Příklad nastavení: (porty 13, 14 a 15 jsou obsluhovány jednou za 10 sekund)

```cpp
const uint8_t ACTIVE_PORTS[] = {13, 14, 15};
const uint16_t UPDATE_INTERVAL = 10000;
```

Než budete kód nahrávat, dejte sondy do květináčů a zalijte je na takovou vlhkost, kterou chcete následně udržovat. Poté kód nahrajte do Arduina a připojte jej k externímu napájecímu zdroji. Dál již bude fungovat samo.

## 3. Controller

- Do zařízení, které bude fungovat jako controller pro modul nahrejte obsluhu serveru.

1. Ujistěte se, že na daném zařízení je k dispozici `nodejs` a `npm`

2. Stáhněte zdrojový kód serveru a spusťe jej

```
git clone https://github.com/hendrychjan/kopretinka_local_server.git
cd kopretinka_local_server
npm i
```

3. Připravte konfiguraci - připravte soubor .env, který bude obsahovat následující:

```
SERVER_URL="http://kopretinka.onrender.com"
PRODUCT_CODE="<kód modulu>"
SERIAL_PORT="<port, ke kterému bude připojeno Arduino>"
```

4. Připojte Arduino pomocí seriové linky, a server spusťte příkazem `npm start`

## 4. Mobilní aplikace

1. Z repozitáře https://github.com/hendrychjan/kopretinka_flutter s v kartě "releases" stáhněte app-release.apk a nainstalujte jej do svého Android zařízení.

2. Zapněte aplikaci. Po prvním spuštění je potřeba připojit se k modulu. Klikněte tedy na ikonku připojení, naskenujte QR kód na Arduinu, a aplikace se připojí.
