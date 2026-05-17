# Zápisnice tímu – ISIC Attendance System

**Miesto stretnutí:** miestnosť A514  
**Termín stretnutí:** pondelok 17:00 (týždenne)  
**Členovia tímu:** Andrian‑Maksym Balaichuk, Danylo Zahorulko, Vladyslav Panik, Vitalii Romaniuk, Yurii Soma  
**Vedúci projektu:** [Ing. Ondrej Gallo, PhD.](https://uim.fei.stuba.sk/pracovnici/ondrej-gallo/), [RNDr. Martin Nehéz, PhD.](https://uim.fei.stuba.sk/pracovnici/nehez/)

---

## Týždeň 1

| | |
|---|---|
| **Dátum** | 22.09.2025 (pondelok) |
| **Čas** | 17:00 – 18:15 |
| **Miesto** | A514 |
| **Účastníci** | Andrian‑Maksym, Danylo, Vladyslav, Vitalii, Yurii |
| **Vypracoval** | Vladyslav Panik |

### Cieľ stretnutia
Zostaviť tím, definovať víziu projektu a identifikovať základné požiadavky.

### Priebeh stretnutia
- Tím sa stretol po prvýkrát a prediskutoval možné témy projektu
- Zhodnotili sa skúsenosti a záujmy jednotlivých členov
- Prebehla diskusia o potrebách cieľových používateľov (študenti, vyučujúci, administratíva)
- Tím jednomyseľne zvolil tému **ISIC Attendance System** – automatizovaný systém evidencie dochádzky pomocou študentských ISIC kariet

### Predbežné rozdelenie rolí

| Člen | Zodpovednosť |
|------|--------------|
| Andrian‑Maksym | Hardware & Embedded Systems |
| Danylo | Backend Developer |
| Vladyslav | UX Designer & Project Coordinator |
| Yurii | Frontend Developer |
| Vitalii | Embedded Software & Libraries |

### Rozhodnutia
- Tému ISIC Attendance System potvrdzujeme
- Komunikačné kanály: MS Teams (chat, porady), zdieľaný Drive (dokumenty)
- Vypracovať a odoslať Ponuku s rozvrhmi a kompetenciami všetkých členov

### Akčné body

| Úloha | Zodpovedný | Termín |
|-------|------------|--------|
| Dodať bio + rozvrh do Ponuky | Všetci | 25.09. |
| Zostaviť a odoslať Ponuku | Vladyslav | 26.09. |
| Identifikovať stakeholderov a ich potreby | Všetci | 26.09. |

> **Zverejnenie zápisnice:** 22.09.2025

---

## Týždeň 2

| | |
|---|---|
| **Dátum** | 29.09.2025 (pondelok) |
| **Čas** | 17:00 – 17:40 |
| **Miesto** | A514 |
| **Účastníci** | Všetci |
| **Vypracoval** | Danylo Zahorulko |

### Zhodnotenie úloh z T1
| Úloha | Stav |
|-------|------|
| Ponuka odoslaná 26.09. | Splnené |
| Bio/rozvrhy | Splnené |
| Stakeholder analýza | Pripravená na diskusiu |

### Cieľ stretnutia
Definovať požiadavky používateľov a vytvoriť počiatočný Product Backlog.

### Priebeh stretnutia
- Vedúci projektu poskytol úvodné usmernenia k rozsahu MVP
- Tím identifikoval hlavných stakeholderov: študenti, vyučujúci, študijné oddelenie
- Definované hlavné User Stories:
  - *„Ako vyučujúci chcem automaticky zaznamenať dochádzku študentov priložením ISIC karty."*
  - *„Ako administrátor chcem exportovať záznamy dochádzky do formátu kompatibilného s AIS."*
  - *„Ako študent chcem vidieť históriu svojej dochádzky."*

### Product Backlog (prvotný návrh)

| # | Položka |
|---|---------|
| 1 | Čítanie ISIC/NFC kariet pomocou hardvérového modulu |
| 2 | Ukladanie záznamov dochádzky do databázy |
| 3 | Webové rozhranie pre správu dochádzky |
| 4 | Export dát do AIS formátu |
| 5 | Autentifikácia a správa používateľov |

### Rozhodnutia
- Začať technickým výskumom v ďalšom týždni
- Zamerať sa na pochopenie existujúcich riešení a technológií

### Akčné body

| Úloha | Zodpovedný | Termín |
|-------|------------|--------|
| Preskúmať možnosti NFC čítačiek | Andrian‑Maksym | 03.10. |
| Analyzovať požiadavky na backend architektúru | Danylo | 03.10. |
| Preskúmať UX best practices pre podobné systémy | Vladyslav | 04.10. |
| Zmapovať možnosti frontend technológií | Yurii | 03.10. |
| Preskúmať dostupné embedded knižnice | Vitalii | 03.10. |

### Retrospektíva
| Čo fungovalo | Čo zlepšiť |
|--------------|------------|
| Rýchla zhoda na téme, efektívna komunikácia, jasné rozdelenie rolí | Detailnejšie špecifikovať kritériá úspešnosti pre jednotlivé úlohy |

> **Zverejnenie zápisnice:** 29.09.2025

---

## Týždeň 3

| | |
|---|---|
| **Dátum** | 06.10.2025 (pondelok) |
| **Čas** | 17:00 – 18:15 |
| **Miesto** | A514 |
| **Účastníci** | Všetci |
| **Vypracoval** | Yurii Soma |

### Zhodnotenie úloh z T2
| Úloha | Stav |
|-------|------|
| Analýza NFC čítačiek | Hotová (PN532 ako preferovaná voľba) |
| Backend požiadavky | Koncept pripravený |
| UX best practices | Zdokumentované |
| Frontend technológie | Porovnanie hotové (preferencia React/TS) |
| Embedded knižnice | Zoznam pripravený |

### Cieľ stretnutia
Vykonať technický výskum a zhodnotiť realizovateľnosť navrhovaných riešení.

### Priebeh stretnutia
- **Andrian‑Maksym** predstavil porovnanie NFC modulov – argumenty pre ESP12‑F + PN532: dostupnosť, Wi‑Fi konektivita, nízka cena, silná komunita
- **Danylo** prezentoval koncept backend architektúry – REST API, relačná databáza
- **Vladyslav** zhrnul UX princípy: jednoduchosť, rýchle filtre, dostupnosť
- **Yurii** porovnal frontend frameworky – React s TypeScriptom ako optimálna voľba
- **Vitalii** analyzoval knižnice pre PN532/ESP – SPI komunikácia ako preferovaný protokol

### Rozhodnutia
- Pokračovať v hlbšej analýze vybraných technológií
- Pripraviť draft architektúry systému

### Akčné body

| Úloha | Zodpovedný | Termín |
|-------|------------|--------|
| Draft elektronickej schémy PN532 ↔ ESP12‑F | Andrian‑Maksym | 09.10. |
| Návrh dátového modelu (entity/vzťahy) | Danylo | 10.10. |
| Návrh UX tokov (dochádzka, export) | Vladyslav | 09.10. |
| Návrh základných obrazoviek | Yurii | 10.10. |
| Porovnanie knižníc/SDK (funkcie, licencie) | Vitalii | 09.10. |

> **Zverejnenie zápisnice:** 06.10.2025

---

## Týždeň 4

| | |
|---|---|
| **Dátum** | 13.10.2025 (pondelok) |
| **Čas** | 17:00 – 17:45 |
| **Miesto** | A514 |
| **Účastníci** | Všetci |
| **Vypracoval** | Vitalii Romaniuk |

### Zhodnotenie úloh z T3
| Úloha | Stav |
|-------|------|
| Draft schémy | Hotový |
| Dátový model | ER diagram pripravený |
| UX toky | Zdokumentované |
| Návrhy obrazoviek | Wireframes pripravené |
| SDK porovnanie | Analýza dokončená |

### Cieľ stretnutia
Finalizovať technologické rozhodnutia a pripraviť sa na fázu návrhu.

### Priebeh stretnutia
- Tím zrevidoval všetky návrhy a analýzy
- Identifikované riziká: dostupnosť komponentov, kompatibilita knižníc
- Definované metriky úspešnosti: latencia čítania < 500ms, 99% úspešnosť čítania

### Technologické rozhodnutia

| Vrstva | Technológia |
|--------|-------------|
| Hardvér | ESP12‑F + PN532 (SPI komunikácia) |
| Backend | FastAPI + PostgreSQL |
| Frontend | React + TypeScript |
| Komunikácia | REST API, prípadne MQTT pre real-time |

### Rozhodnutia
- Pripraviť podklady na objednávku hardvéru
- Zostať v analytickej fáze do konsolidácie všetkých návrhov

### Akčné body

| Úloha | Zodpovedný | Termín |
|-------|------------|--------|
| Finalizovať BOM (Bill of Materials) | Andrian‑Maksym | 15.10. |
| Spísať API kontrakty (OpenAPI koncept) | Danylo | 17.10. |
| Doplniť IA a UX microflows | Vladyslav | 17.10. |
| Pripraviť komponentovú štruktúru FE | Yurii | 17.10. |
| Dokumentácia obmedzení knižníc | Vitalii | 15.10. |

### Retrospektíva
| Čo fungovalo | Čo zlepšiť |
|--------------|------------|
| Dôkladný výskum, kvalitné podklady pre rozhodnutia | Skoršia identifikácia rizík, paralelizácia úloh |

> **Zverejnenie zápisnice:** 13.10.2025

---

## Týždeň 5

| | |
|---|---|
| **Dátum** | 20.10.2025 (pondelok) |
| **Čas** | 17:00 – 18:00 |
| **Miesto** | A514 |
| **Účastníci** | Všetci |
| **Vypracoval** | Andrian‑Maksym Balaichuk |

### Zhodnotenie úloh z T4
| Úloha | Stav |
|-------|------|
| BOM | Finalizovaný a odsúhlasený |
| API kontrakty | Koncept pripravený |
| IA/UX microflows | Dokončené |
| FE komponenty | Štruktúra navrhnutá |
| Dokumentácia knižníc | Kompletná |

### Cieľ stretnutia
Objednať hardvérové komponenty a pripraviť vývojové prostredia.

### Priebeh stretnutia
- **Andrian‑Maksym** uskutočnil objednávku HW komponentov
- Tím zadefinoval testovacie scenáre pre hardvér po doručení
- Dohodnuté pravidlá pre verzionovanie a Git workflow

### Objednané komponenty

| Komponent | Množstvo |
|-----------|----------|
| ESP12‑F modul | 2 ks |
| PN532 NFC modul | 2 ks |
| USB-TTL adaptér | 1 ks |
| Prepojovacie káble a príslušenstvo | — |

### Rozhodnutia
- Po doručení HW zorganizovať montážny workshop
- Každý člen pripraví lokálne vývojové prostredie

### Akčné body

| Úloha | Zodpovedný | Termín |
|-------|------------|--------|
| Sledovať stav zásielky | Andrian‑Maksym | priebežne |
| Pripraviť lokálne BE prostredie | Danylo | 22.10. |
| Pripraviť checklist HW testov | Vitalii | 22.10. |
| Pripraviť šablónu pre testovacie scenáre | Vladyslav | 23.10. |
| Nastaviť FE vývojové prostredie | Yurii | 23.10. |
| Opraviť webstránku pre zápisnice | Yurii | 23.10. |

> **Zverejnenie zápisnice:** 20.10.2025

---

## Týždeň 6

| | |
|---|---|
| **Dátum** | 27.10.2025 (pondelok) |
| **Čas** | 17:00 – 18:10 |
| **Miesto** | A514 |
| **Účastníci** | Všetci |
| **Vypracoval** | Vladyslav Panik |

### Zhodnotenie úloh z T5
| Úloha | Stav |
|-------|------|
| Zásielka HW | Na ceste, očakávané doručenie tento týždeň |
| BE prostredie | Pripravené |
| HW checklist | Kompletný |
| Testovacie šablóny | Pripravené |
| FE prostredie | Nakonfigurované |

### Cieľ stretnutia
Uzavrieť prípravnú fázu a pripraviť sa na implementáciu.

### Priebeh stretnutia
- Konsolidácia všetkých návrhov a dokumentácie
- Definované metodiky pre meranie výkonnosti hardvéru
- Plán pre prvé merania po doručení HW
- Aktualizácia projektovej webstránky

### Dokončené výstupy Discovery fázy

| Výstup | Popis |
|--------|-------|
| Elektronická schéma | Zapojenie ESP12‑F ↔ PN532 |
| Dátový model | Entity, vzťahy, API kontrakty |
| UX toky | Wireframes pre hlavné obrazovky |
| Technická dokumentácia | Výber technológií s odôvodnením |

### Rozhodnutia
- Po doručení HW začať s montážou a prvými testami
- Paralelne spustiť vývoj backend a frontend scaffoldingu

### Akčné body

| Úloha | Zodpovedný | Termín |
|-------|------------|--------|
| Potvrdiť prevzatie HW a zvolať montáž | Andrian‑Maksym | po doručení |
| Pripraviť šablónu reportu z meraní | Vladyslav | 30.10. |
| Vytvoriť backend scaffolding | Danylo | 30.10. |
| Vytvoriť frontend scaffolding | Yurii | 30.10. |
| Pripraviť testovacie prostredie pre HW | Vitalii | 30.10. |

### Retrospektíva
| Čo fungovalo | Čo zlepšiť |
|--------------|------------|
| Efektívna príprava, všetky prostredia pripravené pred príchodom HW | Lepšie odhadnúť dodacie lehoty pri objednávkach |

> **Zverejnenie zápisnice:** 27.10.2025

---

## Týždeň 7

| | |
|---|---|
| **Dátum** | 03.11.2025 (pondelok) |
| **Čas** | 17:00 – 17:35 |
| **Miesto** | A514 |
| **Účastníci** | Všetci |
| **Vypracoval** | Danylo Zahorulko |

### Zhodnotenie úloh z T6
| Úloha | Stav |
|-------|------|
| HW doručený a zmontovaný | Splnené |
| Šablóna reportu z meraní | Pripravená |
| Backend scaffolding | Vytvorený |
| Frontend scaffolding | Vytvorený |
| HW testovacie prostredie | Pripravené |

### Cieľ stretnutia
Spustiť prvé implementačné práce na všetkých vrstvách systému.

### Priebeh stretnutia
- **Andrian‑Maksym** informoval o úspešnej montáži hardvéru a prvých testoch napájania
- **Vitalii** začal prácu s knižnicami pre PN532, skúma formát dát. Prvotne testoval RTOS prístup
- **Danylo** pracuje na backend architektúre a lokálnej verzii
- **Yurii** pokračuje v nastavovaní frontend prostredia
- **Vladyslav** finalizuje UX návrhy

### Stav implementácie

| Komponent | Stav | Poznámka |
|-----------|------|----------|
| Hardvér | Zmontovaný | Prvé testy napájania OK |
| Backend | Scaffolding | Lokálne prostredie funkčné |
| Frontend | Scaffolding | Dev server beží |
| UX | Návrhy | Wireframes v revízii |

### Rozhodnutia
- Pokračovať v testovaní hardvéru a výskume formátu ISIC kariet
- Začať implementáciu základných API endpointov

### Akčné body

| Úloha | Zodpovedný | Termín |
|-------|------------|--------|
| Prvé čítanie ISIC kariet | Andrian‑Maksym, Vitalii | 07.11. |
| Návrh databázovej schémy | Danylo | 07.11. |
| Routing a základná navigácia | Yurii | 07.11. |
| Low-fidelity wireframes | Vladyslav | 07.11. |

> **Zverejnenie zápisnice:** 03.11.2025

---

## Týždeň 8

| | |
|---|---|
| **Dátum** | 10.11.2025 (pondelok) |
| **Čas** | 17:00 – 17:50 |
| **Miesto** | A514 |
| **Účastníci** | Všetci |
| **Vypracoval** | Yurii Soma |

### Zhodnotenie úloh z T7
| Úloha | Stav |
|-------|------|
| Prvé čítanie ISIC | Úspešné, formát analyzovaný |
| DB schéma | Implementovaná |
| FE routing | Funkčný |
| Wireframes | Dokončené |

### Cieľ stretnutia
Rozbehnúť komunikáciu medzi HW a BE, pokračovať vo vývoji všetkých vrstiev.

### Priebeh stretnutia
- **Andrian‑Maksym + Vitalii** dosiahli prvé úspešné čítanie ISIC kariet – UID sa číta správne
- **Dôležité zistenie HW:** Pôvodný RTOS prístup bol príliš pomalý pre runtime operácie. Tím sa rozhodol prejsť na **signal/slot architektúru s task schedulerom**, čo výrazne zlepšilo odozvu systému
- **Danylo** implementoval základné API endpointy pre používateľov
- **Yurii** nastavil routing a základnú navigáciu
- **Vladyslav** dokončil low-fidelity wireframes pre hlavné obrazovky

### Technické zistenia

| Zistenie | Detail |
|----------|--------|
| Protokol kariet | Štandardný MIFARE |
| UID karty | 7-bajtový |
| Dosah čítania | Spoľahlivé do ~3 cm |
| Architektúra | RTOS nevyhovoval → prechod na signal/slot model |

### Rozhodnutia
- Implementovať MQTT komunikáciu medzi HW a BE
- Začať prácu na high-fidelity wireframes
- Definitívne opustiť RTOS v prospech task schedulera

### Akčné body

| Úloha | Zodpovedný | Termín |
|-------|------------|--------|
| MQTT klient na ESP (signal/slot) | Andrian‑Maksym, Vitalii | 14.11. |
| MQTT broker a integrácia do BE | Danylo | 14.11. |
| Základné layouty stránok | Yurii | 14.11. |
| High-fidelity wireframes | Vladyslav | 14.11. |

### Retrospektíva
| Čo fungovalo | Čo zlepšiť |
|--------------|------------|
| Rýchly prechod do implementácie, úspešné čítanie kariet | Lepšia koordinácia medzi HW a BE tímami |

> **Zverejnenie zápisnice:** 10.11.2025

---

## Týždeň 9

| | |
|---|---|
| **Dátum** | 17.11.2025 (pondelok) |
| **Čas** | 17:00 – 18:00 |
| **Miesto** | A514 |
| **Účastníci** | Všetci |
| **Vypracoval** | Vitalii Romaniuk |

### Zhodnotenie úloh z T8
| Úloha | Stav |
|-------|------|
| MQTT na ESP | Implementovaný so signal/slot architektúrou |
| MQTT integrácia BE | Funkčná |
| FE layouty | Základné stránky hotové |
| Hi-fi wireframes | V procese |

### Cieľ stretnutia
Dosiahnuť funkčnú komunikáciu HW → BE a rozvinúť core funkcionalitu.

### Priebeh stretnutia
- **Andrian‑Maksym + Vitalii** úspešne implementovali MQTT klienta s task schedulerom – správy sa odosielajú pri každom prečítaní karty. Signal/slot architektúra sa osvedčila
- **Danylo** integroval MQTT broker, backend prijíma správy a ukladá do DB
- **Yurii** vytvoril základné stránky – dashboard, zoznam používateľov
- **Vladyslav** pokračuje na high-fidelity wireframes

### MQTT Protokol

| Parameter | Hodnota |
|-----------|---------|
| Topic | `isic/attendance/{room_id}` |
| Payload | `{"uid": "...", "timestamp": "..."}` |
| QoS | 1 (at least once) |

### Rozhodnutia
- Implementovať CRUD operácie pre používateľov a dochádzku
- Začať prácu na registračnom formulári

### Akčné body

| Úloha | Zodpovedný | Termín |
|-------|------------|--------|
| Testovanie stability HW | Andrian‑Maksym, Vitalii | 21.11. |
| CRUD pre users/attendance | Danylo | 21.11. |
| Formulár registrácie používateľa | Yurii | 21.11. |
| Dokončiť hi-fi wireframes | Vladyslav | 21.11. |

> **Zverejnenie zápisnice:** 17.11.2025

---

## Týždeň 10

| | |
|---|---|
| **Dátum** | 24.11.2025 (pondelok) |
| **Čas** | 17:00 – 17:45 |
| **Miesto** | A514 |
| **Účastníci** | Všetci |
| **Vypracoval** | Andrian‑Maksym Balaichuk |

### Zhodnotenie úloh z T9
| Úloha | Stav |
|-------|------|
| Stabilita HW | Testovaná, identifikované problémy s napájaním |
| CRUD operácie | Implementované |
| Registračný formulár | Funkčný |
| Hi-fi wireframes | Dokončené |

### Cieľ stretnutia
Stabilizovať HW, dokončiť backend API, integrovať FE s BE.

### Priebeh stretnutia
- **Andrian‑Maksym + Vitalii** identifikovali problémy so stabilitou napájania – potrebná optimalizácia pre produkčné nasadenie
- **Danylo** dokončil všetky CRUD operácie, DB plne funkčná
- **Yurii** implementoval formulár pre registráciu, zobrazenie zoznamu používateľov
- **Vladyslav** dokončil high-fidelity wireframes, pripravil handoff pre FE

### Identifikované problémy

| Problém | Popis |
|---------|-------|
| Napájanie | Nestabilita pri dlhodobom behu |
| Power management | Potrebná optimalizácia pre produkciu |

### Rozhodnutia
- Riešiť problémy s napájaním pre produkčné nasadenie
- Prepojiť FE s BE API

### Akčné body

| Úloha | Zodpovedný | Termín |
|-------|------------|--------|
| Optimalizácia napájania | Andrian‑Maksym, Vitalii | 28.11. |
| Autentifikácia API | Danylo | 28.11. |
| Integrácia s BE API | Yurii | 28.11. |
| Review wireframes s tímom | Vladyslav | 28.11. |

### Retrospektíva
| Čo fungovalo | Čo zlepšiť |
|--------------|------------|
| Rýchly progress na BE a FE, kvalitné wireframes, signal/slot architektúra sa osvedčila | Skoršie testovanie HW stability |

> **Zverejnenie zápisnice:** 24.11.2025

---

## Týždeň 11 — Vianočná pauza

| | |
|---|---|
| **Dátum** | 22.12.2025 (pondelok) |
| **Čas** | 17:00 – 17:20 |
| **Miesto** | A514 |
| **Účastníci** | Všetci |
| **Vypracoval** | Vladyslav Panik |

### Zhodnotenie úloh z T10
| Úloha | Stav |
|-------|------|
| Optimalizácia napájania | Čiastočne vyriešená, potrebné ďalšie testy |
| API autentifikácia | Implementovaná |
| FE + BE integrácia | Základná funkčná |
| Wireframes review | Dokončený, feedback zapracovaný |

### Cieľ stretnutia
Krátka synchronizácia pred vianočnými sviatkami, plánovanie práce na január.

### Priebeh stretnutia
- Krátke zhodnotenie stavu projektu pred pauzou
- Tím si stanovil individuálne ciele na sviatočné obdobie (voliteľné)
- Dohodnutý termín ďalšieho stretnutia po Novom roku

### Stav pred pauzou

| Komponent | Stav |
|-----------|------|
| Hardvér | Funkčný, OTA a power management rozpracované |
| Backend | Plne funkčný, všetky endpointy hotové |
| Frontend | Integrácia s BE OK, základné obrazovky |
| UX | Wireframes kompletné |

### Rozhodnutia
- Pauza v aktívnom vývoji počas sviatkov
- Individuálna práca voliteľná
- Ďalšie stretnutie 06.01.2026

### Akčné body

| Úloha | Zodpovedný | Termín |
|-------|------------|--------|
| *Voliteľne:* OTA research | Andrian‑Maksym, Vitalii | 06.01. |
| *Voliteľne:* Bug fixes BE | Danylo | 06.01. |
| *Voliteľne:* UI polish | Yurii | 06.01. |
| Pripraviť agendu na január | Vladyslav | 06.01. |

> **Zverejnenie zápisnice:** 22.12.2025

---

## Týždeň 12

| | |
|---|---|
| **Dátum** | 06.01.2026 (pondelok) |
| **Čas** | 17:00 – 17:45 |
| **Miesto** | A514 |
| **Účastníci** | Všetci |
| **Vypracoval** | Danylo Zahorulko |

### Zhodnotenie úloh z T11
| Úloha | Stav |
|-------|------|
| OTA research | Základná implementácia hotová |
| BE bug fixes | Dokončené |
| UI polish | Čiastočne |
| Agenda na január | Pripravená |

### Cieľ stretnutia
Zhodnotenie aktuálneho stavu projektu po vianočnej pauze, plánovanie ďalších krokov.

### Priebeh stretnutia
- Krátke zhodnotenie stavu projektu po pauze
- **Andrian‑Maksym + Vitalii** implementovali základný OTA mechanizmus, problémy s napájaním pretrvávajú
- **Danylo** opravil identifikované bugy v backende
- **Yurii** pokračoval v polish UI
- **Vladyslav** pripravil agendu na január

### Rozhodnutia
- Prioritizovať opravu napájania HW
- Dokončiť FE podľa wireframes
- Pripraviť backend pre čítanie backdoor údajov

### Akčné body

| Úloha | Zodpovedný | Termín |
|-------|------------|--------|
| Optimalizácia napájania HW | Andrian‑Maksym, Vitalii | 10.01. |
| Dokončiť FE podľa wireframes | Yurii | 10.01. |
| Pripraviť reader pre backdoor | Danylo | 10.01. |
| Koordinácia prípravy PDF dokumentácie | Vladyslav | 10.01. |

### Retrospektíva
| Čo fungovalo | Čo zlepšiť |
|--------------|------------|
| Úspešná integrácia všetkých vrstiev, fungujúce end-to-end demo | Skoršia identifikácia HW problémov |

> **Zverejnenie zápisnice:** 06.01.2026

---

## Týždeň 13

| | |
|---|---|
| **Dátum** | 13.01.2026 (pondelok) |
| **Čas** | 17:00 – 18:00 |
| **Miesto** | A514 |
| **Účastníci** | Všetci |
| **Vypracoval** | Yurii Soma |

### Zhodnotenie úloh z T12
| Úloha | Stav |
|-------|------|
| Optimalizácia napájania HW | Dokončená |
| FE podľa wireframes | Dokončený |
| Reader pre backdoor | Implementovaný |
| Koordinácia PDF dokumentácie | V procese |

### Cieľ stretnutia
Finalizácia hlavných komponentov systému, príprava dokumentácie.

### Priebeh stretnutia
- **Andrian‑Maksym + Vitalii** dokončili optimalizáciu napájania – HW teraz stabilne funguje v produkčnom režime
- **Andrian‑Maksym** + **Yurii** prevzali implementáciu readera pre backdoor (Danylo neprítomný)
- **Yurii** dokončil frontend podľa wireframes, všetky obrazovky sú funkčné
- **Vladyslav** koordinoval tím pri príprave PDF dokumentácie projektu
- *Poznámka: Danylo Zahorulko bol na tomto stretnutí neprítomný*

### Stav projektu

| Komponent | Stav |
|-----------|------|
| Hardvér | Plne funkčný, napájanie optimalizované |
| Backend | Kompletný vrátane backdoor readera |
| Frontend | Dokončený podľa wireframes |
| Dokumentácia | PDF v príprave |

### Rozhodnutia
- Pripraviť finálnu prezentáciu projektu
- Dokončiť PDF dokumentáciu
- Vykonať end-to-end testovanie

### Akčné body

| Úloha | Zodpovedný | Termín |
|-------|------------|--------|
| Finálne testovanie HW | Andrian‑Maksym, Vitalii | 17.01. |
| Finalizácia BE dokumentácie | Danylo | 17.01. |
| Testovanie FE | Yurii | 17.01. |
| Dokončenie PDF dokumentácie | Vladyslav | 17.01. |

### Retrospektíva
| Čo fungovalo | Čo zlepšiť |
|--------------|------------|
| Úspešné dokončenie všetkých hlavných komponentov, efektívna koordinácia | Viac času na testovanie pred finalizáciou |

> **Zverejnenie zápisnice:** 13.01.2026

---

## Týždeň 14

| | |
|---|---|
| **Dátum** | 20.01.2026 (utorok) |
| **Čas** | 10:00 – 11:00 |
| **Miesto** | A514 |
| **Účastníci** | Všetci |
| **Vypracoval** | Vitalii Romaniuk |

### Zhodnotenie úloh z T13
| Úloha | Stav |
|-------|------|
| Finálne testovanie HW | Dokončené |
| BE: reader pre backdoor | Dokončený (Andrian‑Maksym + Yurii) |
| Testovanie FE | Dokončené |
| Dokončenie PDF dokumentácie | V procese |

### Cieľ stretnutia
Definovanie nového plánu: rozšírenie systému o role (učiteľ, admin) a podporu viacerých HW zariadení.

### Priebeh stretnutia
- **Tím** prediskutoval nové požiadavky — role-based prístup (admin, učiteľ, študent) a podpora viacerých HW čítačiek naraz
- **Andrian‑Maksym + Yurii** navrhli architektúru role-based systému v BE a FE — rozdelenie logiky podľa roly
- **Andrian‑Maksym** identifikoval kritický bug: PN532 sleep mód spôsobuje zaseknutie čítačky po určitom čase nečinnosti
- **Vladyslav** aktualizoval backlog s novými úlohami

### Stav projektu

| Komponent | Stav |
|-----------|------|
| Hardvér | Funkčný; identifikovaný PN532 sleep bug |
| Backend | Kompletný; plánované rozšírenie rolí |
| Frontend | Kompletný; plánované nové views pre role |
| Dokumentácia | PDF v procese |

### Rozhodnutia
- Implementovať role-based systém (admin, učiteľ) v BE a FE
- Pridať podporu viacerých HW zariadení v BE
- Opraviť PN532 sleep bug v HW

### Akčné body

| Úloha | Zodpovedný | Termín |
|-------|------------|--------|
| Návrh BE architektúry pre role | Andrian‑Maksym, Yurii | 03.02. |
| Štart FE role views | Yurii | 03.02. |
| Analýza a oprava PN532 sleep bug | Andrian‑Maksym, Vitalii | 03.02. |
| Aktualizácia dokumentácie k novým features | Vladyslav | 03.02. |

### Retrospektíva
| Čo fungovalo | Čo zlepšiť |
|--------------|------------|
| Rýchla identifikácia nových požiadaviek po testovaní | Skôr riešiť PN532 sleep problém |

> **Zverejnenie zápisnice:** 20.01.2026

---

## Týždeň 15

| | |
|---|---|
| **Dátum** | 03.02.2026 (utorok) |
| **Čas** | 10:00 – 11:00 |
| **Miesto** | A514 |
| **Účastníci** | Všetci |
| **Vypracoval** | Vladyslav Panik |

### Zhodnotenie úloh z T14
| Úloha | Stav |
|-------|------|
| Návrh BE architektúry pre role | Dokončený |
| Štart FE role views | Zahájený |
| Analýza PN532 sleep bug | Dokončená; fix v procese |
| Aktualizácia dokumentácie | V procese |

### Cieľ stretnutia
Implementácia admin panela a HW konzoly; zavedenie nového config formátu pre lepšiu latenciu; HW panel interakcia.

### Priebeh stretnutia
- **Yurii** implementoval admin panel vo FE — správa zariadení, používateľov, skenov
- **Andrian‑Maksym + Yurii** spustili BE endpointy pre HW konzolu (konfigurácia zariadení, vzdialená správa) a REST API pre HW panel interakcie
- **Andrian‑Maksym + Vitalii** pridali podporu nového JSON config formátu pre HW — latencia skenov sa znížila o ~40 %; implementovaná HW panel interakcia cez MQTT
- **Vladyslav** testoval admin panel end-to-end a zaznamenal spätnú väzbu

### Stav projektu

| Komponent | Stav |
|-----------|------|
| Hardvér | Nový config formát aktívny; PN532 sleep fix pokračuje |
| Backend | Admin API + HW konzola funkčné |
| Frontend | Admin panel implementovaný |
| Dokumentácia | Admin panel zdokumentovaný |

### Rozhodnutia
- Integrovať HW konzolu do FE admin sekcie
- Pridať MQTT status topic pre online/offline monitoring HW zariadení

### Akčné body

| Úloha | Zodpovedný | Termín |
|-------|------------|--------|
| Integrácia HW konzoly do FE | Yurii | 17.02. |
| Implementovať MQTT heartbeat na HW | Andrian‑Maksym | 17.02. |
| BE logika pre sledovanie online/offline stavu | Andrian‑Maksym, Yurii | 17.02. |
| FE indikátor stavu zariadenia | Yurii | 17.02. |

### Retrospektíva
| Čo fungovalo | Čo zlepšiť |
|--------------|------------|
| Efektívna práca v pároch; admin panel hotový v jednom šprinte | Paralelizovať BE a FE prácu skôr |

> **Zverejnenie zápisnice:** 03.02.2026

---

## Týždeň 16

| | |
|---|---|
| **Dátum** | 17.02.2026 (utorok) |
| **Čas** | 10:00 – 11:00 |
| **Miesto** | A514 |
| **Účastníci** | Všetci |
| **Vypracoval** | Yurii Soma |

### Zhodnotenie úloh z T15
| Úloha | Stav |
|-------|------|
| Integrácia HW konzoly do FE | Dokončená |
| MQTT heartbeat na HW | Dokončený |
| BE logika online/offline stavu | Dokončená |
| FE indikátor stavu zariadenia | Dokončený |

### Cieľ stretnutia
Overenie MQTT status topic; komplexné end-to-end testovanie; optimalizácia systému.

### Priebeh stretnutia
- **Andrian‑Maksym + Vitalii** overili MQTT heartbeat na viacerých zariadeniach — online/offline detekcia funguje správne
- **Andrian‑Maksym + Yurii** vykonali globálne end-to-end testovanie všetkých features
- Testovanie odhalilo: **ESP12‑F modul má nedostatočný WiFi signál pre WPA2 eduroam** — RAM modulu nestačí na WPA2‑Enterprise šifrovanie
- **Vladyslav** konzultoval situáciu s vedúcim projektu — vedúci potvrdil možnosť produkčného nasadenia v STU
- Rozhodnuté: pre produkciu prejsť na **ESP32**, optimalizovať firmware

### Stav projektu

| Komponent | Stav |
|-----------|------|
| Hardvér | Status monitoring funkčný; ESP12‑F nevhodný pre eduroam — prechod na ESP32 |
| Backend | Kompletný; optimalizácie prebehli |
| Frontend | Admin panel + online/offline indikátory funkčné |
| Dokumentácia | Aktualizovaná |

### Rozhodnutia
- ESP12‑F nahradiť ESP32 pre produkčné nasadenie
- Optimalizovať HW firmware pre ESP32
- Opraviť bugy v BE a FE; opraviť PN532 knižnicu

### Akčné body

| Úloha | Zodpovedný | Termín |
|-------|------------|--------|
| Optimalizácia HW kódu pre ESP32 | Andrian‑Maksym, Vitalii | 03.03. |
| Oprava kolízie skenov v BE | Andrian‑Maksym, Yurii | 03.03. |
| FE: pridanie študenta + pravý klik skratky | Yurii | 03.03. |
| Oprava PN532 lib timeout / segfault | Andrian‑Maksym, Vitalii | 03.03. |

### Retrospektíva
| Čo fungovalo | Čo zlepšiť |
|--------------|------------|
| Globálne testovanie včas odhalilo WiFi limitáciu HW | Skôr testovať HW na cieľovej WiFi infraštruktúre |

> **Zverejnenie zápisnice:** 17.02.2026

---

## Týždeň 17

| | |
|---|---|
| **Dátum** | 03.03.2026 (utorok) |
| **Čas** | 10:00 – 11:00 |
| **Miesto** | A514 |
| **Účastníci** | Všetci |
| **Vypracoval** | Andrian‑Maksym Balaichuk |

### Zhodnotenie úloh z T16
| Úloha | Stav |
|-------|------|
| Optimalizácia HW kódu pre ESP32 | Dokončená |
| Oprava kolízie skenov v BE | Dokončená |
| FE: pridanie študenta + skratky | Dokončené |
| Oprava PN532 lib timeout / segfault | Dokončená |

### Cieľ stretnutia
Verifikácia všetkých opráv; finálne prípravy pred produkčným nasadením.

### Priebeh stretnutia
- **Andrian‑Maksym + Vitalii** opravili kritický problém v PN532 knižnici: sleep mód knižnice nie je kompatibilný s jej vlastným volaním — spôsoboval segmentation fault a nadmerný timeout; implementované vlastné riešenie bez lib sleep
- **Andrian‑Maksym + Vitalii** dokončili optimalizáciu ESP32 firmvéru — stabilnejší WiFi, lepší power management
- **Andrian‑Maksym + Yurii** refaktorovali BE scanning logiku pre robustnejšie spracovanie edge cases pri kolízii skenov
- **Yurii** dokončil FE: funkcia pridania nového študenta, skratky pravého kliku v tabuľke, optimalizácia renderingu pre veľké datasety
- **Vladyslav** pripravil deployment plán a checklist

### Stav projektu

| Komponent | Stav |
|-----------|------|
| Hardvér | PN532 lib fix hotový; ESP32 firmware pripravený na produkciu |
| Backend | Collision fix + robustná scanning logika |
| Frontend | Pridanie študenta, skratky, rendering optimalizácia |
| Dokumentácia | Deployment checklist pripravený |

### Rozhodnutia
- Pristúpiť k produkčnému nasadeniu na server STU

### Akčné body

| Úloha | Zodpovedný | Termín |
|-------|------------|--------|
| Príprava produkčného BE deploymentu | Andrian‑Maksym, Yurii | 17.03. |
| Konfigurácia ESP32 pre STU WiFi | Andrian‑Maksym | 17.03. |
| FE produkčný build | Yurii | 17.03. |
| Deployment dokumentácia | Vladyslav | 17.03. |

### Retrospektíva
| Čo fungovalo | Čo zlepšiť |
|--------------|------------|
| Efektívna oprava PN532 lib; systém plne pripravený na nasadenie | Skôr riešiť kompatibilitu knižníc s HW módmi |

> **Zverejnenie zápisnice:** 03.03.2026

---

## Týždeň 18

| | |
|---|---|
| **Dátum** | 17.03.2026 (utorok) |
| **Čas** | 10:00 – 11:00 |
| **Miesto** | A514 |
| **Účastníci** | Všetci |
| **Vypracoval** | Yurii Soma |

### Zhodnotenie úloh z T17
| Úloha | Stav |
|-------|------|
| Príprava produkčného BE deploymentu | Dokončená |
| Konfigurácia ESP32 pre STU WiFi | Dokončená |
| FE produkčný build | Dokončený |
| Deployment dokumentácia | Dokončená |

### Cieľ stretnutia
Produkčné nasadenie celého systému na server STU.

### Priebeh stretnutia
- **Andrian‑Maksym + Yurii** nasadili BE na STU server; databáza a MQTT broker nakonfigurované
- **Yurii** nahrál FE build na produkčný server; aplikácia dostupná
- **Andrian‑Maksym + Vitalii** nakonfigurovali ESP32 zariadenia pre STU sieť — všetky sa úspešne pripojili
- Systém **funguje end-to-end** v produkčnom prostredí STU
- Identifikované 2 problémy: OTA aktualizácie blokované verejným API (firewall STU); HW problém pri inicializácii pri slabom WiFi signáli — zariadenie zlyhá namiesto opakovaného pokusu

### Stav projektu

| Komponent | Stav |
|-----------|------|
| Hardvér | Nasadený; WiFi init bug pri slabom signáli |
| Backend | Nasadený na STU server; funkčný |
| Frontend | Nasadený; verejne dostupný |
| Dokumentácia | Deployment guide hotový |

### Rozhodnutia
- Opraviť OTA mechanizmus — náhrada verejného API za lokálne riešenie
- Opraviť HW init logiku — retry pri zlyhaní WiFi pripojenia

### Akčné body

| Úloha | Zodpovedný | Termín |
|-------|------------|--------|
| Oprava OTA (lokálne riešenie) | Andrian‑Maksym, Yurii | 31.03. |
| HW fix: retry logika pri WiFi init zlyhaní | Andrian‑Maksym, Vitalii | 31.03. |
| Testovanie opráv v produkčnom prostredí | Všetci | 31.03. |
| Aktualizácia deployment dokumentácie | Vladyslav | 31.03. |

### Retrospektíva
| Čo fungovalo | Čo zlepšiť |
|--------------|------------|
| Úspešné produkčné nasadenie; tím efektívne pracoval | Skôr testovať OTA mechanizmus v produkčnom prostredí |

> **Zverejnenie zápisnice:** 17.03.2026

---

## Týždeň 19

| | |
|---|---|
| **Dátum** | 31.03.2026 (utorok) |
| **Čas** | 10:00 – 11:00 |
| **Miesto** | A514 |
| **Účastníci** | Všetci |
| **Vypracoval** | Vitalii Romaniuk |

### Zhodnotenie úloh z T18
| Úloha | Stav |
|-------|------|
| Oprava OTA | Dokončená |
| HW fix: retry pri WiFi init | Dokončený |
| Testovanie opráv | Dokončené |
| Aktualizácia deployment dokumentácie | Dokončená |

### Cieľ stretnutia
Komplexné testovanie so všetkými opravami; overenie OTA funkčnosti v produkcii.

### Priebeh stretnutia
- **Andrian‑Maksym + Vitalii** otestovali nový OTA mechanizmus — aktualizácie fungujú správne cez lokálne OTA riešenie
- **Andrian‑Maksym + Yurii** vykonali regresné testy BE a FE — žiadne regresie
- **Všetci** overili HW retry logiku — zariadenie sa teraz opätovne pripája pri nestabilnom WiFi signáli
- OTA a HW connection problémy **potvrdené ako opravené**

### Stav projektu

| Komponent | Stav |
|-----------|------|
| Hardvér | OTA funkčné; WiFi retry opravený; produkčne stabilný |
| Backend | Plne funkčný, nasadený |
| Frontend | Plne funkčný, nasadený |
| Dokumentácia | Aktualizovaná |

### Rozhodnutia
- Systém je stabilný; prechod do fázy dokumentácie a finálnej prezentácie

### Akčné body

| Úloha | Zodpovedný | Termín |
|-------|------------|--------|
| Finálna PDF dokumentácia — technická sekcia | Andrian‑Maksym, Yurii | 28.04. |
| Finálna PDF dokumentácia — HW sekcia | Andrian‑Maksym, Vitalii | 28.04. |
| Finálna PDF dokumentácia — FE sekcia | Yurii | 28.04. |
| Koordinácia a štruktúra dokumentácie | Vladyslav | 28.04. |

### Retrospektíva
| Čo fungovalo | Čo zlepšiť |
|--------------|------------|
| Rýchla oprava oboch produkčných problémov; systém plne stabilný | Automatizovať OTA testovanie v CI/CD |

> **Zverejnenie zápisnice:** 31.03.2026

---

## Týždeň 20

| | |
|---|---|
| **Dátum** | 28.04.2026 (utorok) |
| **Čas** | 10:00 – 11:00 |
| **Miesto** | A514 |
| **Účastníci** | Všetci |
| **Vypracoval** | Vladyslav Panik |

### Zhodnotenie úloh z T19
| Úloha | Stav |
|-------|------|
| Finálna dokumentácia — technická sekcia | Dokončená |
| Finálna dokumentácia — HW sekcia | Dokončená |
| Finálna dokumentácia — FE sekcia | Dokončená |
| Koordinácia dokumentácie | Dokončená |

### Cieľ stretnutia
Finalizácia dokumentácie; príprava finálnej prezentácie a demo scenára.

### Priebeh stretnutia
- **Vladyslav** prezentoval štruktúru finálnej dokumentácie PDF — tím schválil
- **Andrian‑Maksym + Yurii** dokončili technickú sekciu (BE architektúra, API, databázový model)
- **Andrian‑Maksym + Vitalii** dokončili HW sekciu (schémy, firmware, konfigurácia ESP32)
- **Yurii** dokončil FE sekciu (screenshots, user stories, UI popis)
- **Tím** pripravil demo scenár pre prezentáciu: ISIC skenovanie → BE spracovanie → FE realtime zobrazenie

### Stav projektu

| Komponent | Stav |
|-----------|------|
| Hardvér | Kompletný, nasadený, produkčne stabilný |
| Backend | Kompletný, nasadený |
| Frontend | Kompletný, nasadený |
| Dokumentácia | ~95 % hotová; finalizácia pred prezentáciou |

### Rozhodnutia
- Prezentovať projekt 20.05.2026
- Generálna skúška end-to-end demo pred prezentáciou

### Akčné body

| Úloha | Zodpovedný | Termín |
|-------|------------|--------|
| Finalizácia PDF dokumentácie | Vladyslav | 19.05. |
| Záverečné doladenie prezentácie | Vladyslav | 19.05. |
| Generálna skúška end-to-end demo | Všetci | 19.05. |

### Retrospektíva
| Čo fungovalo | Čo zlepšiť |
|--------------|------------|
| Všetky ciele projektu splnené; koordinovaná finálna fáza | Začať dokumentáciu skôr v priebehu projektu |

> **Zverejnenie zápisnice:** 28.04.2026

---

## Týždeň 21 — Finálna prezentácia

| | |
|---|---|
| **Dátum** | 20.05.2026 (streda) |
| **Čas** | 10:00 – 11:30 |
| **Miesto** | A514 |
| **Účastníci** | Všetci |
| **Vypracoval** | Vitalii Romaniuk |

### Zhodnotenie úloh z T20
| Úloha | Stav |
|-------|------|
| Finalizácia PDF dokumentácie | Dokončená, odovzdaná |
| Záverečné doladenie prezentácie | Dokončené |
| Generálna skúška end-to-end demo | Úspešná |

### Štatistika projektu

| Metrika | Hodnota |
|---------|---------|
| **Celkový počet stretnutí** | 21 |
| **Trvanie projektu** | 22.09.2025 – 20.05.2026 (~8 mesiacov) |
| **Počet submodulov** | 4 (Frontend, Backend, Hardware, Dokumentácia) |

### Cieľ stretnutia
Finálna prezentácia projektu ISIC Attendance System; uzavretie projektu.

### Priebeh stretnutia
- **Vladyslav** otvoril záverečné stretnutie a prezentoval históriu projektu
- **Andrian‑Maksym + Vitalii** predviedli HW: živé skenovanie ISIC kariet, OTA aktualizácia, online/offline monitoring zariadení
- **Andrian‑Maksym + Yurii** prezentovali BE architektúru, REST API, MQTT flow, produkčné nasadenie na STU server
- **Yurii** predviedol FE: admin panel, učiteľský dashboard, role systém, realtime zobrazenie skenov
- **Celý tím** úspešne predviedol end-to-end demo: ISIC karta → NFC čítačka → MQTT → BE → FE

### Stav projektu

| Komponent | Stav |
|-----------|------|
| Hardvér | Produkčne nasadený, plne funkčný |
| Backend | Produkčne nasadený, plne funkčný |
| Frontend | Produkčne nasadený, plne funkčný |
| Dokumentácia | Kompletná, odovzdaná |

### Rozhodnutia
- Projekt je **ukončený** — všetky pôvodné aj rozšírené požiadavky splnené

> **Zverejnenie zápisnice:** 20.05.2026

---

## Teória zápisníc — zásady pre tím

| Zásada | Popis |
|--------|-------|
| **Bezodkladné vyhotovenie** | Zápis vyhotoviť hneď po porade, ideálne v ten istý deň |
| **Stručný formát** | Pre týždenné porady používať jasné rozhodnutia a úlohy |
| **Vyhodnotenie úloh** | Na začiatku každej porady vyhodnotiť úlohy z predchádzajúceho stretnutia |
| **Zverejnenie** | Zápisnice zverejňovať na web minimálne 1 deň pred ďalším stretnutím |
| **Obsah** | Dátum, čas, miesto, účastníci, autor, cieľ, priebeh, rozhodnutia, akčné body |
