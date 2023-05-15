# Zpráva 

## Připojení periferií a komunikace v systému

K Arduinu, jsou připojeny dva multiplexory. Jeden na vlhkostní sondy, druhý na čerpadla. Arduino udržuje informaci o vlhkosti na jednotlivých sondách, a pokud je někde hranice nižší, tak pustí čerpadlo, dokud se nevrátí na původní hodnotu. Informace ze sond jsou odesílány na server, odkud si je může uživatel stáhnout a zobrazit v mobilní aplikaci.

## Univerzalita - připojení více uživatelů

Aby mohlo systém využívat více uživatelů, každý modul je při "výrobě" označen kódem, který ho identifikuje, a kterým se modul následně autentifikuje serveru (je pro každý modul unikátní a dostatečně složitý na to, aby se dal uhádnout. Aby uživatel nemusel kód nikde opisovat, je v Arduinu zapsán natrvrdo, a na krabičce přípravku je qr kód, který se naskenuje v mobilní aplikaci.

## Konfigurace

Konfigurace měla být pomocí SD karty, ale to je zbytečně drahé a složité. Lepší nápad byl v konfiguračním režimu nastavit Modul jako wifi AP, připojit se mobilní aplikací a jednoduše konfiguraci poslat. Řešení s ESP8266 se ale nepodařilo realizovat, takže konfigurace se provádí v kódu pro Arduino.

## Životnost vlhkostních sond

Jakmile jsou sondy připojeny k napájení (přitom ani nemusí zrovna docházet ke čtení), prochází jimi stejnosměrné napětí, což způsobuje efekt elektrolýzy na nožičkách sondy. Jelikož použité vlhkostní sondy nejsou odolné proti korozi, tento způsob použití významně snižuje jejich životnost. Proto jsem je připojil všechny na společné napájení, které je pomocí tranzistoru ovládáni Arduinem. Napájení se zapne pouze při měření, v ostatních případech je pak vypnuto, a u sond proto nedochází k degradaci.

## Napájení čerpadel

Jelikož použitá ponorná vodní čerpadla mají větší odběr proudu, než dokáže Arduino poskytnout, bylo potřeba je napájet pomocí externího zdroje. K tomu byl využit zdroj, který napájí již arduino, jen bylo potřeba pomocí stabilizátoru napětí snížit napětí na hodnotu, která čerpadlům vyhovuje. Čerpadla jsou pak Arduinem ovládána pomocí tranzistorů.

## ESP8266

Modul měl komunikovat pomocí TCP v rámci wifi sítě. Ke komunikaci byl zvolen lowcost chip ESP8266, který dokáže fungovat jako server (pro konfiguraci), tak jako klient (běžný pracovní režim). Tento chip se ale ukázal jako největší technologický problém celého projektu. Je připojen pomocí softwarové seriové linky, na které při přenosu vzniká tak moc rušení, že to použití úplně znemožnilo. Zde je potřeba provést další vývoj, zaměřit se na odstínění přenosových kabelů, snížení baud rate přímo ve firmware čipu a ideálně vymyslet, jak pro komumikaci využít hardwarovou seriovou linku (během vývoje je zabraná). Pro eliminaci chyb jsem si napsal vlastní "protokol", který využívá kontrolní součty, ale ani to nepomohlo. Někdy ESP nebylo schopně přenést ani informaci o tom, zda se klient odpojil, a spojení ukončil.

Nakonec tedy nezbylo, než pro komunikaci použít extra prvek - Raspberry Pi, připojené usb kabelem sériovou linkou.

## Závěr

Pracovat na projektu byla opravdu zábava, i když práce s ESP8266 byla chvílemi značně frustrující. 

Narozdíl od mých předchozích projektů s Raspberry Pi bylo potřeba jít trochu více do hloubky elektrotechniky. S Raspberry Pi mi stačilo jednoduše připojovat hotové moduly pomocí komunikačních linek, ale u Kopretinky jsem musel pracovat s některými koncepty, které jsem sice poznal teoreticky, ale nikdy se s nimi nesetkal při reálné práci. Často mě zachraňovaly znalosti z předmětu BI-TZP z minulého semestru.

Zpětně musím říct, že jsem celý projekt hodně podcenil. Výsledný modul sice zvládá základní funkcionality, ale zdaleka není v takovém stavu, jak jsem si při původním návrhu představoval. Zjistil jsem, že při práci s HW se člověk často setká s problémy, které by vůbec nečekal (například ono rušení na sériové lince), a vyřešit je trvá někdy překvapivě dlouho.

Doma mám spoustu komponent, a v hlavě spoustu nápadů, jak celý přípravek vylepšit. Na vývoji chci do budoucna určitě pokračovat, implementovat všechnny dosavadně chybící funkcionality a především vyřešit problém s ESP. Ve výsledku bude mít přípravek reálné využití.