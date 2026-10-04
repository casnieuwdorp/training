# Training

Een mobiele, offline trainings- en voedingstracker als één los HTML-bestand. Geen build, geen framework, geen backend. Je opent `index.html` en alles draait lokaal in de browser, met je data opgeslagen op het apparaat zelf.

De app is gebouwd rond een lean bulk naar 100 kg met chest als prioriteit: 3x full body (losse gewichten + kabels voor flyes, max 60 min) afgewisseld met 3 runs, zondag rust. Blok 1 start op 5 oktober 2026.

## Inhoud van de repo

| Bestand | App | Omschrijving |
|---|---|---|
| `index.html` | **Training** | Hoofd-app. Amber thema, 3x full body + 3 runs, progressie-coach, bulk-macro's (3.500 krachtdag / 3.800 loopdag). |
| `fit-app.html` | **Fit** | Losse tweede app. Roze thema, Pilates/Upper/Core/Hyrox/Billen en Benen. Eigen opslag en lagere macro's. |

De twee apps staan los van elkaar: ze gebruiken een eigen opslagsleutel, dus hun data loopt niet door elkaar.

## Snel starten

Geen installatie nodig.

1. Open `index.html` in een mobiele browser (Safari of Chrome).
2. Optioneel: "Zet op beginscherm" voor een fullscreen, app-achtige ervaring. De app is als PWA geconfigureerd (standalone, eigen titel en statusbalkkleur).
3. Begin met loggen. Alles wordt automatisch lokaal bewaard.

## Functies

### Training
- Dagtabs, opent op vandaag. Weeknummer van het blok bovenin.
- **Progressie-coach per oefening:** double progression (alle sets op de bovenkant van de range → gewicht omhoog), opbouwfase voor squat en RDL (+5 kg/week tot 100 en 115 kg), fases voor dips en pull-ups (eigen gewicht → met gewicht bij 3 x 8). Het voorgestelde gewicht staat als placeholder in het veld; afvinken zonder invoer neemt het voorstel over.
- Chest-oefeningen: 6 weken geen e1RM-progressie → seintje om een variatie te kiezen.
- "Laatste keer" zoekt over alle dagen heen, dus historie blijft bewaard als een oefening van dag wisselt.
- Volgorde = prioriteit (chest altijd eerst), één spier tegelijk. Supersets alleen op dezelfde plek (drukke gym). Warming-up per krachtdag, chest-sets deze week.
- Deficit push-ups ontgrendelen in week 4, deloadbanner in week 6 en 12.
- Runs: minuten, km, gemiddelde hartslag, automatisch tempo, doelduur per week en een vinkje voor de zone 2-check.

### Dashboard
- Gewicht: 7-daags gemiddelde, tempo vs +0,25 kg/week met advies (+/−150 kcal), verwachte weken tot 100 kg. Eén meting per dag.
- Doelen eind december: bench 80 x 8, incline DB 30 x 8, dips en pull-ups 3 x 8 met eigen gewicht.
- Chest-volume per week (laatste 6 weken), zone 2-tempo-grafiek met de 7:00-lijn, laatste runs.
- **Backup:** exporteer als JSON (deelmenu op iPhone) en importeer weer. Herinnering na 7 dagen zonder backup.

### Voeding
- Doelen per dag: krachtdag/zondag 3.500 kcal (P185 K500 V85), loopdag 3.800 kcal (P185 K575 V85).
- Dagcijfer, waarbij calorieën in een bulk zwaarder wegen.
- Consistentie: gemiddelde afwijking van je calorie-doel over de laatste 7 gelogde dagen.

### Progressie
- Grafiek per oefening op e1RM (gewicht en reps samen), of reps voor eigen-gewicht-oefeningen.
- Laatste sessie per workout: gedane vs geplande sets.

## Trainingsschema

| Dag | Sessie |
|---|---|
| Ma | Full body A: incline DB, cable fly, pull-ups, squat, ab wheel, lateral raise |
| Di | Run 1: kort en rustig |
| Wo | Full body B: bench, dips, chest-supported row, RDL, back extension, hanging leg raise, calf raise |
| Do | Run 2: comfortabel + strides |
| Vr | Full body C: low-incline DB, low-to-high cable fly, deficit push-ups, one-arm row, cossack squat, dead bug + Copenhagen plank, armen |
| Za | Run 3: langste run |
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
- Macro's: `MACROS_LIFT` en `MACROS_RUN`. Doelgewicht: `GOAL_BW`. Blokstart: `PROGRAM_START`. Loopopbouw: `RUN_PLAN`. 12-wekendoelen: `GOALS`.
