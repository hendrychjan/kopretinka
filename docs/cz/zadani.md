# Zahradní systém Kopretinka

Modulární zařízení v kompaktní podobě, které sleduje vlhkost půdy z připojených sond, a uživateli poskytuje přehled v mobilní aplikaci. K dispozici bude možnost připojit pár čerpadel, která bude uživatel moci zapojit do automatizovaného procesu zavlažování.

Hlavní cíl, který projekt sleduje, je aby zažízení dokázal oživit a používat i technicky neznalý uživatel. Dále také, aby se jednalo o systém, který je univerzální pro všechny uživatele, a nemusí se v něm při výrobě provádět žádná uživateli specifická konfigurace.

## Nastavení

Zařízení bude fungovat ve dvou režimech - konfigurace, a běžný režim.

### Konfigurační režim

Zařízení bude fungovat jako server, ke kterému se prostřednictvím wifi uživatel připojí svým mobilním telefonem.

V konfiguračním režimu uživatel definuje aktivní porty (sondy a čerpadla), nastaví pravidla automatizace (při jaké hranici vlhkosti se má aktivovat zavlažování), a nakonec připojí zařízení k wifi. 

### Běžný režim

V tomto režimu zařízení pracuje podle předem definované konfigurace, a periodicky prostřednictvím internetu sdílí statistická data (aktuální vlhkost, akce automatizace), která si uživatel může zobrazit v mobilní aplikaci.

## Bezpečnost

Důležitou funkcionalitou je také ochrana proti havárii zavlažování. Detektor vlhkosti, který v případě, že zjistí přítomnost vody, zajistí vypnutí všech čerpadel.