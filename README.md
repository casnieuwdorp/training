# Training

Een mobiele, offline trainings- en voedingstracker als één los HTML-bestand. Geen build, geen framework, geen backend. Je opent `index.html` en alles draait lokaal in de browser, met je data opgeslagen op het apparaat zelf.

Blok 2 (vanaf november 2026): recomp naar 95 kg shredded. 3x full body (max 60 min, max 3 sets per oefening, focus bovenkant borst, geen deadlifts) afgewisseld met 3 runs, zondag rust. Loopschema: 8 weken (intervallen di, rustig + strides do, lange run za), gebaseerd op de Runkeeper-export van 1 nov 2026, eindigend met een 5 km-test. Blok 2 start op 2 november 2026.

## Inhoud van de repo

| Bestand | App | Omschrijving |
|---|---|---|
| `index.html` | **Training** | Hoofd-app. Amber thema, 3x full body + 3 runs, progressie-coach, recomp-macro's (3.200 kcal per dag). |
| `fit-app.html` | **Fit** | Losse tweede app. Roze thema, Pilates/Upper/Core/Hyrox/Billen en Benen. Eigen opslag en lagere macro's. |

De twee apps staan los van elkaar: ze gebruiken een eigen opslagsleutel, dus hun data loopt niet door elkaar.

## Snel starten

Geen installatie nodig.

1. Open `index.html` in een mobiele browser (Safari of Chrome).
2. Optioneel: "Zet op beginscherm" voor een fullscreen, app-achtige ervaring. De app is als PWA geconfigureerd (standalone, eigen titel en statusbalkkleur).
3. Begin met loggen. Alles wordt automatisch lokaal bewaard.

## Functies

### Training
- Dagtabs, opent op vandaag. Weeknummer van het blok en een tijdschatting per krachtsessie (doel 45 tot 60 min) bovenin.
- **Progressie-coach per oefening:** double progression (alle sets op de bovenkant van de range → gewicht omhoog), opbouwfase voor squat en RDL (+5 kg/week tot 100 en 115 kg), fases voor dips en pull-ups (eigen gewicht → met gewicht bij 3 x 10). Het voorgestelde gewicht staat als placeholder in het veld; afvinken zonder invoer neemt het voorstel over.
- Chest-oefeningen: 6 weken geen e1RM-progressie → seintje om een variatie te kiezen.
- "Laatste keer" zoekt over alle dagen heen, dus historie blijft bewaard als een oefening van dag wisselt.
- Max 3 sets per oefening, binnen het uur. Supersets alleen op dezelfde plek (drukke gym). Warming-up per krachtdag, chest-sets deze week.
- Deloadbanner elke 6 weken.
- Runs: minuten, km, gemiddelde hartslag, automatisch tempo, de sessie van die week uit het 8-wekenschema (`RUN_PLAN`) en een waarschuwing als een rustige run boven HR 158 zat (max HR ~200). Plus het vinkje voor de zone 2-check.

### Dashboard
- Gewicht: 7-daags gemiddelde met recomp-advies (stabiel tot +0,2 kg/week, anders +200 of -150 kcal). Doel 95 kg shredded. Eén meting per dag.
- Doelen eind december: incline smith 70 x 8, squat 100 x 6, dips en pull-ups 3 x 10 met eigen gewicht.
- Chest-volume per week (laatste 6 weken), zone 2-tempo-grafiek met de 7:00-lijn, laatste runs.
- **Backup:** exporteer als JSON (deelmenu op iPhone) en importeer weer. Herinnering na 7 dagen zonder backup.
- **Automatische backup:** koppel op het dashboard een privé GitHub-repo met een fine-grained token (alleen Contents: read/write op die ene repo). Elke wijziging wordt na 15 sec opgeslagen in `data/latest.json`, en elke week krijgt een eigen bestand `data/weeks/week-JJJJ-MM-DD.json`. Het token staat alleen lokaal op het apparaat (sleutel `gym_sync`) en gaat nooit mee in exports. Openbare repo's worden geweigerd.

### Voeding
- Doel per dag: 3.200 kcal (P190 K420 V85), kalibreren op het weekgemiddelde gewicht.
- Dagcijfer, waarbij eiwit en calorieën het zwaarst wegen.
- Consistentie: gemiddelde afwijking van je calorie-doel over de laatste 7 gelogde dagen.

### Progressie
- Grafiek per oefening op e1RM (gewicht en reps samen), of reps voor eigen-gewicht-oefeningen.
- Laatste sessie per workout: gedane vs geplande sets.

## Trainingsschema

| Dag | Sessie |
|---|---|
| Ma | Full body A: squat, incline smith press, barbell row, RDL, lateral raise, incline curl + overhead triceps, hanging leg raise |
| Di | Intervallen (800 m tot 2 km herhalingen) |
| Wo | Full body B: Bulgarian split squat, staande shoulder press, pull-ups, low-incline DB press, back extension, reverse fly, calf raise, cable crunch |
| Do | Rustig (HR < 150) + strides |
| Vr | Full body C: hack squat, dips, lat pulldown, cable row, leg curl, lateral raise, hammer curl + pushdown, seated calf raise |
| Za | Lange run, vanaf week 3 met snel slot; week 8 een 5 km-test |
| Zo | Rust |

## Techniek

- Eén `index.html`, vanilla JavaScript, geen externe libraries (alleen het lettertype DM Sans via Google Fonts).
- Persistentie via `localStorage`. De Training-app gebruikt sleutel `gym_v3` (ongewijzigd, bestaande logs blijven staan), de Fit-app gebruikt `fit_v1`.
- Datums in lokale tijd (niet UTC).
- Grafieken zijn handgemaakte inline-SVG, dus zonder chart-library.
- Mobiel-eerst, donker thema, maximale breedte 480 px, amber accent (`#C8873A`).
- Screen Wake Lock API om het scherm aan te houden tijdens een sessie.

## Data en privacy

Alle data staat uitsluitend lokaal in de browser van het apparaat. Er gaat niets naar een server. Dat betekent ook: wis je je browsergegevens of gebruik je een ander apparaat, dan begint de log opnieuw. Daarom zit er een export/import-functie in op het dashboard. Maak elke week een backup.

## Aanpassen

- Het schema staat bovenin het script in `SCHEMA`. Per oefening: `id`, `block`, `name`, `sets`, `min`/`max` (reprange), `rest`, `start` (startgewicht), `inc` (stap), optioneel `rampTo`, `mode` (`bw` of `core`), `chest`, `ss` (superset), `fromWeek`, `alias` (oude id's voor historie).
- Macro's: `MACROS_LIFT` en `MACROS_RUN`. Doelgewicht: `GOAL_BW`. Blokstart: `PROGRAM_START`. Loopschema: `RUN_PLAN` (per week [di, do, za] met titel, uitleg en km). 12-wekendoelen: `GOALS`.
