# Kopretinka

## Zpracování ze strany hardware

Nejdůležitějšími použitými moduly jsou Arduino NANO, vlhkoměry, ponorná vodní čerpadla a WiFi modul ESP8266.

### Arduino NANO

Celý systém je řízen vývojovou deskou Arduino. Pro výsledný produkt je použit model NANO, a to z důvodu minimalizace velikosti výsledného přípravku.

### Vlhkoměry

Pro měření vlhkosti půdy je použit jednoduchý vlhkoměr od firmy ESES, který je připojen ke komparátoru LM393. Jeho výhodou je nízká cena, ale nevýhodou to, že není opatřen proti korozi, tedy je potřeba měřit pouze periodicky, a jinak mít senzor úplně odpojený (jinak na elektrodách sondy dochází k elektrolýze, což způsobuje jejich degradaci). Jelikož všechny senzory jsou zapojeny na společné VIN a GND, vodič VIN je přerušen NPN tranzistorem, který je ovládán digitálním pinem Arduina, a tím lze ovládat připojení/odpojení sond od napájení, čímž lze prodloužit životnost. Analogové vývody z komparátorů jednotlivých diod jsou přivedeny do multiplexoru, který je připojen k Arduinu.

### Vodní čerpadla

Pro zalití v případě překročení hranice sucha jsou použitá jednoduchá mini ponorná čerpadla. Jelikož odběr čerpadla může být až 200 mA, ale maximální povolený proud na digitálním pinu Arduina je 90 mA, musí být čerpala připojena ke společnému napájecímu pinu Arduina - 5V. Aby je bylo možné ovládat, na napájecí vodič je každému čerpadlu připojen NPN tranzistor, který lze sepnout/rozepnout pomocí digitálních pinů Arduina. Čerpadla jsou ponořena v nádrži s vodou, a od nich vedou silikonové hadičky do jednotlivých květináčů. Pro případ poruchy v čerpadle jsou pro ochranu tranzistorů a Arduina do zapojení přidány ochranné diody.

### Multiplexory

Připojovat jednotlivé senzory a ovládání čerpadel přímo na piny Arduina by bylo vysoce neefektivní, šlo by pak kontrolovat maximálně 6 květináčů. Protože není potřeba používat více čerpadel/senzorů najednou v jednu chvíli, hodí se použití multiplexorů. Díky tomu můžeme celou aplikaci rozšířit pro až 16 květináčů.

### WiFi modul

Pro účely komunikace Arduina s uživatelem je použit modul ESP8266, který funguje jako klient, nebo server. V režimu serveru se nachází, když je celý přípravek v konfiguračním modu. Jako klient je nastaven po uživatelské konfiguraci. Modul je k Arduinu připojen přes sériovou linku, ale protože funguje pouze na logické úrovni 3.3V, je potřeba předřadit měnič, který sníží 5V z Arduina na požadovaných 3.3V.

## Architektura, způsob komunikace

Je snaha dosažení univerzality, tedy předpokládá se, že systém může používat více uživatelů. Uživatelé ovládají zařízení mobilní aplikací, kde mají přehled o hodnotách naměřených sondami, a mohou zařízení konfigurovat (hranici sucha pro zalévání,  apod.). Každé zařízení má svoje unikátní ID. Data rozlišena tímto ID posílá na rozhraní REST API na společném webovém serveru. Odtud je následně čte uživatel, který si ve svém mobilním zařízení nastaví ID zařízení, jehož data chce sledovat. Systém pro jednoduchost není opatřen žádnou autentizaci pomocí uživatelských účtů, předpokládá se, že ID zařízení bude navrženo tak, aby nebylo možné jej uhodnout.

Pro urychlení vývoje jsou použity technologie Node.js (server) a Flutter (mobilní klient).

## Konfigurace, konfigurační režim

Na začátku se zařízení nachází v konfiguračním režimu. Každé zařízení má v sobě pevně zadané ID již z "výroby". Je potřeba nastavit připojení k WiFi a informovat desku, na kterých pinech jsou připojeny jaké sondy a čerpadla. Proto se celý systém může nacházet ve dvou stavech - konfigurační, a pracovní. Mezi nimi se lze jednoduše přepínat pomocí fyzického tlačítka na přípravku. Současný stav je uživateli signalizován pomocí LED diody. V konfiguračním režimu Arduino nastaví svůj WiFi modul do režimu serveru (AP). Uživatel se na něj připojí, a v aplikaci přípravku pošle připravenou konfiguraci. Po jejím přijetí se Arduino přepne do pracovního režimu. Jednotlivé kroky nastavení budou podrobněji popsány v manuálu použití.

## Pracovní režim

Přípravek v uživatelem zadaném intervalu provede následující:

1. Aktivace napájení sond
2. Přečtení hodnoty na každé sondě
3. Odeslání naměřené hodnoty na server
4. Pokud je překročena hodnota sucha, a uživatel povolil automatické zalití, zapne se čerpadlo a zalévá tak dlouho, dokud není dosažena požadovaná hodnota vlhkosti, nakonec se odešlě informace o provedení zalití
5. Vypnutí sond
6. Přechod do režimu spánku za účelem dosažení minimální spotřeby.
