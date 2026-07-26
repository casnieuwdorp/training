# Training

Een mobiele, offline trainings- en voedingstracker als één los HTML-bestand. Geen build, geen framework, geen backend. Je opent `index.html` en alles draait lokaal in de browser, met je data opgeslagen op het apparaat zelf.

De app is gebouwd rond een 5-daagse krachtsplit (Push/Pull/Upper/Legs & Abs/Lower) met 2 losse cardiodagen (interval en Zone 2 hardlopen) en cut-macro's, gericht op lean blijven met progressie op kracht en volume. Compound lifts staan op 3 sets, isolatie op 2-3 sets (meestal 2), zodat een sessie rond de 45 minuten blijft.

## Inhoud van de repo

| Bestand | App | Omschrijving |
|---|---|---|
| `index.html` | **Training** | Hoofd-app. Amber thema, 5-daagse Push/Pull/Legs/Upper/Lower plus een conditie/HIIT-dag. Cut-macro's. |
| `fit-app.html` | **Fit** | Losse tweede app. Roze thema, Pilates/Upper/Core/Hyrox/Billen en Benen. Eigen opslag en lagere macro's. |

De twee apps staan los van elkaar: ze gebruiken een eigen opslagsleutel, dus hun data loopt niet door elkaar.

## Snel starten

Geen installatie nodig.

1. Open `index.html` in een mobiele browser (Safari of Chrome).
2. Optioneel: "Zet op beginscherm" voor een fullscreen, app-achtige ervaring. De app is als PWA geconfigureerd (standalone, eigen titel en statusbalkkleur).
3. Begin met loggen. Alles wordt automatisch lokaal bewaard.

## Functies

### Training
- Dagtabs voor de hele week. Opent standaard op de dag van vandaag.
- Per oefening een kaart met sets, reps en geadviseerde rusttijd.
- Per set een invoer voor gewicht en reps, met een afvink-knop.
- "Laatste keer": de vorige sessie van diezelfde oefening wordt als referentie getoond.
- PR-badge per oefening, gebaseerd op het zwaarste gewicht maal reps.
- Superset-aanduiding: oefeningen met een 🔗 horen bij elkaar, met de naam van de partneroefening erbij.
- Loggbare cardiokaarten (eigen blauwe stijl) met één invoerveld, voor de optionele Zone 2 en de HIIT-dag.
- Automatische feedback na het afvinken: herkent een PR, meer reps bij hetzelfde gewicht, een lichtere sessie (met een hersteltip), of een gelijke sessie met een nudge voor de volgende keer.
- Coachingsnotitie per dag met de focus en aandachtspunten.
- Rustdag toont een apart herstelscherm.
- Screen Wake Lock: tijdens het trainen blijft het scherm aan. Een groen stipje rechtsboven geeft aan dat dit actief is.

### Dashboard
- Kerncijfers: aantal gelogde sessies, huidig lichaamsgewicht, doelgewicht (90 kg) en het verschil daarmee.
- Lichaamsgewicht loggen met een mini-grafiek van de laatste metingen.
- Overzicht van persoonlijke records over alle krachtoefeningen.

### Voeding
- Dagdoelen: 2500 kcal, 195 g eiwit, 265 g koolhydraten, 75 g vet.
- Eiwit, koolhydraten, vet en calorieën loggen, plus stappen (doel 8000). Calorieën worden automatisch berekend uit de macro's als je ze niet zelf invult.
- Voortgangsbalken per macro en gedetailleerde feedbackregels (te laag, op schema, te hoog).
- Dagcijfer van 1 tot 10, gewogen opgebouwd: eiwit 25 punten, calorieën 20, koolhydraten 10, vet 5 en stappen 20. Met een label van "Zwakke dag" tot "Perfecte dag".
- Geschiedenis van de laatste 7 dagen.

### Progressie
- Kies een oefening voor een PR-badge en een grafiek van het gewichtsverloop over je sessies.
- Wekelijks volume per trainingsdag, op basis van de afgevinkte sets in de laatste sessie.

## Trainingsschema (Training-app)

| Dag | Type | Focus |
|---|---|---|
| Maandag | Push | Chest, schouders (standing barbell press), triceps (buitenste en lange hoofd) |
| Dinsdag | Pull | Rug, biceps (beide hoofden), achterdelts |
| Woensdag | Legs & Abs + Zone 2 | Ochtend: quad-dominant beenwerk en buikspieren. Middag: rustige duurloop |
| Donderdag | Upper | Tweede borstsessie, rug-dikte, achterdelts, resterende biceps-hoofden |
| Vrijdag | Lower | Hamstrings, glutes, kuiten, obliques |
| Zaterdag | Interval Cardio | Interval hardlopen voor conditie |
| Zondag | Rust | Herstel |

Alle drie de triceps-hoofden (buitenste: pushdown, lange: overhead extension + skull crusher, mediale: tricep dips) en beide biceps-hoofden (plus brachialis) komen verspreid over Push, Pull en Upper aan bod. Supersets staan alleen nog waar ze met dezelfde apparatuur zonder omlopen te doen zijn (Donderdag: dumbbell skull crusher + dumbbell hammer curl). Lateral raise en standing calf raise zijn van 3 naar 2 sets gegaan, zodat er ruimte was om bench dips toe te voegen zonder de sessies langer te maken dan nodig.

## Techniek

- Eén `index.html`, vanilla JavaScript, geen externe libraries (alleen het lettertype DM Sans via Google Fonts).
- Persistentie via `localStorage`. De Training-app gebruikt sleutel `gym_v3`, de Fit-app gebruikt `fit_v1`.
- Grafieken zijn handgemaakte inline-SVG, dus zonder chart-library.
- Mobiel-eerst, donker thema, maximale breedte 480 px, amber accent (`#C8873A`).
- Screen Wake Lock API om het scherm aan te houden tijdens een sessie.

## Data en privacy

Alle data staat uitsluitend lokaal in de browser van het apparaat. Er gaat niets naar een server. Dat betekent ook: wis je je browsergegevens of gebruik je een ander apparaat, dan begint de log opnieuw. Een export- of synchronisatiefunctie zit er nu niet in.

## Aanpassen

- Het schema staat bovenin het script in het `SCHEMA`-object. Per dag een lijst met oefeningen, elk met `id`, `name`, `sets`, `reps` en `rest` (in seconden). Een superset markeer je met `ss`, een cardio-item met `cardio:true`.
- De macrodoelen staan in het `MACROS`-object.
- Het doelgewicht (90 kg) staat in het dashboardgedeelte.
