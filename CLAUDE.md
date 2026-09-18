# JO17-1 site

Kleine statische site met de speelafspraken van een JO17-elftal. Publiek toegankelijk
voor spelers en ouders, en bedoeld om per seizoen aangevuld te worden.

## Uitgangspunten

- Gewone HTML, CSS en JavaScript. Geen framework, geen build-stap, geen npm.
  De site moet over twee jaar nog werken en aanpasbaar zijn door iemand die geen
  ontwikkelaar is.
- Alles in het Nederlands, ook variabelen en functienamen in de JavaScript.
- Het team zegt **drukzetten**, niet jagen. Gebruik die term overal.
- Mobile first. De meeste bezoekers openen dit op een telefoon vanuit de groepsapp.
- Één gedeelde stylesheet in `assets/site.css`. Uitzondering is
  `speelafspraken.html`, die is bewust zelfstandig zodat hij ook los te delen is.
- Bewegingen worden altijd door de gebruiker gestart, nooit automatisch bij het laden.
  `prefers-reduced-motion` respecteren.

## Hoe het eruit ziet

Licht ontwerp op papierwit, met het veld als enige grote kleurvlak. De kleuren staan
als variabelen bovenin `assets/site.css` en, omdat die pagina zelfstandig is, ook
bovenin `speelafspraken.html`. Pas ze op beide plekken aan.

- papier `#f8f7f4`, kaarten wit met rand `#e5e7eb`, tekst `#1f2937`, gedempt `#6b7280`
- veld `#4a9d63` zonder banen, lijnen wit op 85 procent
- eigen spelers `#1d4ed8`, keeper `#eab308`, tegenstander `#9ca3af`
- geel `#fbbf24` voor looppijlen en voor het kader met de stem in het veld,
  dat kader heeft vlak `#fef3c7` en tekst `#92400e`

Op het veld betekent geel een loopactie en wit met streepjes een pass. Tekstjes op
het gras zijn wit en vet, met een dun donker randje zodat ze op het groen leesbaar
blijven. Boven het veld staat één grijze regel met de context van de stap.

## Privacy, belangrijk

Er staan minderjarige spelers achter dit project.

- Elke pagina heeft `<meta name="robots" content="noindex, nofollow">` in de head,
  zodat de site niet in Google komt. Een nieuwe pagina krijgt die regel ook. Er staat
  bewust geen blokkade in een `robots.txt`: dan kan Google de noindex niet lezen.
- Voornamen bij rugnummers mogen, en staan op `opstelling.html`. Dat is een
  bewuste keuze van de trainer, met de ouders geinformeerd.
- Wat er niet op mag: achternamen, foto's, wie tweede keus of vervanger is,
  individuele beoordelingen, en alles over blessures of herstel. Het bronmateriaal
  van de staf bevat dat wel, dus neem daar nooit een blok in zijn geheel over.
- In de speelafspraken blijven we naar rugnummers en posities verwijzen, ook in het
  kader met de stem in het veld. Een naam voegt daar niets toe aan de afspraak.
- Wedstrijdanalyses, opstellingen en alles wat over individuele spelers gaat is
  stafmateriaal en hoort niet op de openbare site.
- Bij twijfel: niet publiceren, eerst overleggen.

**Let op: deze repo is publiek** (github.com/michielotterloo/abs17-1), en dit bestand
wordt meegepusht. Er staat dus niets in over blessures, herstel, beoordelingen of
wie tweede keus is, ook niet als het handig zou zijn bij het plannen.

Dat soort stafmateriaal staat in de map `staf/`, en die staat in `.gitignore`. Daar
hoort in: blessures en beschikbaarheid, wedstrijdanalyses, en alles wat over een
individuele speler gaat. Daar staat ook `oefeningen.md`: een index van de trainingsvormen
die we gebruiken, met per vorm de bron, bij hoeveel spelers hij werkt, en of hij met onze
ene keeper kan. Oefeningen van de KNVB en van TrainersMagazine zijn andermans materiaal,
dus daar staat een verwijzing, geen kopie. Zet er nooit iets uit die map in een bestand dat wel
gepusht wordt.

## Speelwijze, context voor de inhoud

Systeem 4-2-3-1, met rugnummers als vaste taal:

- 1 keeper, 2 rechtsback, 3 linksback, 4 en 5 centraal achterin
- 6 en 8 op het middenveld, 10 als aanvallende middenvelder
- 7 rechtsbuiten, 11 linksbuiten, 9 spits

Twee regels per linie, dat is het hart van de site. De centrale verdediger is de
roeper voor de organisatie en voor het zakken. Het startsein voor drukzetten ligt bij
de 9, en alleen op een trigger; de 6 dirigeert het blok. Dat volgt afspraak 1 en 4 uit
het teamplan.

## De pagina's

- `index.html`: startpagina met kaarten naar de rest
- `teamafspraken.html`: de zes afspraken die voor het hele elftal gelden, plus wie wat roept
- `opstelling.html`: voornaam per rugnummer in de 4-2-3-1, alleen de basiself
- `speelafspraken.html`: de twee regels per linie, geanimeerd. Drie momenten: balbezit,
  balverlies en de pressingval bij de uittrap van de tegenstander
- `per-linie.html`: per linie de losse situaties, met fout en goed naast elkaar
- `schaduwspel.html`: hoe het blok meeschuift met de bal, in drie delen
- `standaardsituaties.html`: corners, vrije trappen en inworpen, vijf vaste beelden
- `trainingen.html`: de vaste warming-up en de kernvormen
- `weekthemas.html`: het thema per week

Op `index.html` staan op dit moment bewust alleen de kaarten naar `teamafspraken.html`,
`speelafspraken.html` en `per-linie.html`. De andere vijf kaarten staan in commentaar in dat bestand, en die
pagina's zijn nog wel gewoon bereikbaar via hun eigen adres.

De drie animatiepagina's zijn bewust zelfstandig, met eigen CSS in het bestand, zodat
ze ook los te delen zijn. Dat betekent wel dat de kleuren op vier plekken staan: in
`assets/site.css` en in elk van die drie pagina's.

## Hoe de tactiekplaat werkt

In `speelafspraken.html` staat bovenin het script een object `scenarios`. Elk
scenario heeft:

- `regels`: de tekst per linie in het paneel rechts
- `stem`: de tekst in het gele kader onder het paneel
- `spelers`: de beginposities, coördinaten in een SVG-viewBox van 480 bij 640,
  eigen doel onderaan
- `stappen`: per stap `kort` voor de stappenlijst, `kop` voor de grijze regel boven
  het veld, `tekst` voor het bijschrift, `bal`, welke `linie` oplicht, en in `pos`
  alleen de spelers die in die stap bewegen

Per stap kan er verder nog bij:

- `labels`: de tekstjes op het gras, als `["tekst", x, y]`, met optioneel een anker
  (`"start"`, `"middle"`, `"end"`) en `"groot"` als vijfde waarde. Elke stap zet de
  hele lijst opnieuw, dus een label dat moet blijven staan herhaal je
- `passes`: witte streepjespijlen, als `[x1, y1, x2, y2]`
- `lijn`: de streepjeslijn van de verdediging, als `[x1, y, x2]`
- `blok`: `true` zet het lichte blok over het veld aan

De gele looppijlen worden zelf getekend tussen de oude en de nieuwe positie, en
alleen voor onze eigen spelers. Ze worden aan beide kanten ingekort zodat de punt
naast de speler blijft staan. De laatste stap van een scenario is bedoeld als de
plaat die je kunt uitprinten: daar staan alle labels op.

Een nieuwe animatie maak je door een scenario toe te voegen en een knop in de
tabbladenrij. Die knop krijgt als id `tab-` plus de naam van het scenario, dan
regelt `kiesScenario` het oplichten zelf; die loopt over alles wat in `scenarios`
staat en hoeft niet aangepast te worden.

Bij het scenario `uittrap` staat het doel van de tegenstander bovenaan in beeld,
want daar ligt de bal. De tegenstander heeft daar zeven spelers, want de val gaat
juist over hun structuur: keeper, twee centrale verdedigers op de lijn, twee backs
en twee pivots.

## Hoe de standaardsituaties werken

`standaardsituaties.html` gebruikt de gedeelde stylesheet en heeft bovenin het
script een object `situaties`. Elk blok heeft een `titel`, een `kop` voor de grijze
regel boven het veld, de balpositie, de `spelers` met hun coordinaten in een
SVG-viewBox van 480 bij 400, de `taken` per rugnummer en een `rest`-tekst over wat
er achterblijft. Geen animatie hier, alleen vaste beelden.

Let op welk doel bovenaan staat: dat is steeds het doel waar in die situatie op
gespeeld wordt. Bij een corner voor en een vrije trap voor is dat hun doel, bij de
rest het onze. Daarom staat de kop per situatie en niet statisch boven het veld.

Een nieuwe situatie maak je door een blok toe te voegen en een knop in de
tabbladenrij met als id `tab-` plus de naam; `kiesSituatie` loopt over alles wat in
`situaties` staat.

## Hoe de weekthema's werken

`weekthemas.html` is gewone HTML, geen script. Elke week is een `article.week`.
De week waar we nu in zitten krijgt `class="week nu"` (geel), een week die
geweest is `class="week gehad"` (gedempt). Bovenin het bestand staat een blok
in commentaar dat je kunt kopieren voor een nieuwe week.

## Wat er nog moet komen

- Standaardsituaties: de vaste nemers zijn nog niet afgesproken. Wat er nu in de
  beelden staat is een voorstel; de strafschopnemer is nog helemaal leeg
- De weekthema's bijhouden per week; de vier die er staan zijn de start van
  het seizoen 2026/2027

## Deploy

Statische hosting op GitHub Pages, geen server nodig. Het bestand `CNAME`
bevat `abs17-1.dutchview.com`, dat is het adres van de site.

Wat er eenmalig moet gebeuren:

1. Repo op GitHub zetten en pushen.
2. In de repo onder Settings, Pages: bron is de branch `main`, map `/`.
3. In Google Cloud DNS (daar staat de DNS van dutchview.com) een CNAME
   toevoegen: `abs17-1` naar `<github-gebruiker>.github.io`. De A-records van
   dutchview.com zelf blijven ongemoeid, die wijzen naar Webflow.
4. Terug in Settings, Pages wachten tot het certificaat klaar is en dan
   Enforce HTTPS aanzetten.

Daarna is elke `git push` naar `main` een publicatie.
