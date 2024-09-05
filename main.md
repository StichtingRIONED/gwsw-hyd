# GWSW-Hyd

<style>
  .symbolSmall{width:20px;height:20px;margin-right:1em;vertical-align:middle}
  .symbol{width:30px;height:30px;margin-right:1em;vertical-align:middle}
</style>

<a id="werkgroep"></a>
De module GWSW-Hyd is ontwikkeld door Jordie Netten (Stichting Rioned/Netten Wateradvies), in samenwerking met de werkgroep GWSW-Hyd, bestaande uit:
- Marco van Bijnen (Van Bijnen Advies)
- Katrin Boden, Guido Vaes en Ilse Bisson (Inneautech, Hydroscan, Infoworks ICM)
- Arthur van Dam en Didrik Meijer (Deltares, D-Hydro)
- Hendrik Kingma (Riodesk)
- Peter van der Kruk (Waternet)
- Thijs Nix (Tauw)
- Perry Pijnappels (Kragten)
- Frank Ramaekers (Gemeente Helmond)
- Arnold van 't Veld en Jonas van Schrojenstein Landman (Nelen & Schuurmans, 3Di)
- Sjaak Verkerk (Antea Group)
- Johan Veurink (Arcadis)
- Marinus Vonhof (Stichting RIONED/Marivon)
- Wouter van Riel (Stichting RIONED/Infralytics)

Vanuit Stichting RIONED is Eric Oosterom de verantwoordelijk projectmanager. Vragen over de module en de totstandkoming en vaststelling ervan kunt u stellen via gwsw@rioned.org. 

Gelieve de inhoudsopgave als leeswijzer te beschouwen.

# Inleiding
voor de uitwisseling van informatie ten behoeve van het uitvoeren van hydraulische berekeningen werd tot 2020 het SUF-HYD (Standaard UitwisselingsFormaat Hydraulische gegevens) van rioolstelsels gebruikt. De rioleringspraktijk vroeg al lang om een actualisatie, die dankzij de ontwikkeling van het GegevensWoordenboek Stedelijk Water heeft geleid tot het GWSW-Hyd. Het GWSW-Hyd beschrijft de ontologie (hoe iets heet) en het datamodel (wat de verbanden zijn tussen die dingen) van de objecten en bijbehorende kenmerken die relevant zijn voor hydraulische berekeningen van rioolstelsels. 

## Gegevensstromen en gereedschappen rond het GWSW

Nu al worden door veel gemeenten en waterschappen de gegevens van rioolstelsels op de GWSW Server gepubliceerd. Deze gegevens komen 1) via de upload functionaliteit van de GWSW Server middels een OroX (.ttl) bestand vanuit het gemeentelijke beheerpakket of 2) via het GegevensKnooppunt Waterschappen (Het Waterschapshuis) terecht in de dataomgeving van de betreffende organisatie op de GWSW Server. 

De module GWSW-Hyd ondersteunt in de gegevensuitwisseling voor het maken van hydraulische berekeningen in het vrijverval stelsel van de riolering. Het is dus van belang dat de OroX (die vanuit het gemeentelijke rioleringsbeheerpakket komt) deze gegevens conform het datamodel GWSW-Hyd bevat. 

Op de GWSW-server worden de gegevens van de verschillende organisaties opgeslagen. Met de applicatie 'Download Hydx' kan er van de dataset op de GWSW-server een uitwisselbestand (GWSW.hydx) wordt aangemaakt en worden gedownload. 

In Figuur 2 staan de gegevensstromen en gereedschappen weergegeven die gebruikt worden voor het maken van hydraulische berekeningen met GWSW-Hyd.

<img src="media/proces_datauitwisseling_hyd.jpg" style="width:100%;height:50%" />

*Figuur 2 - Gegevensstromen en gereedschappen voor het maken van hydraulische berekeningen*  

# Datamodel GWSW-Hyd

## Algemeen
De benodigde informatie voor de uitwisseling van informatie over de riolering wordt samengevat in meerdere .CSV bestanden die tezamen het GWSW.hydx vormen. Deze bestanden zijn gebaseerd op de onderstaande informatiecategorieën:
- Netwerk
- Belasting
- Meta-informatie

Hieruit komen de volgende .CSV bestanden voort:
- KNOOPPUNT.csv
- VERBINDING.csv
- KUNSTWERK.csv
- OPPERVLAK.csv
- DEBIET.csv
- META.csv

In de bovenstaande .CSV bestanden kan worden verwezen naar de onderstaande definities, die als losse .CSV bestanden (kunnen) worden bijgevoegd:
- ITOBJECT.csv
- PROFIEL.csv
- NWRW.csv
- VERLOOP.csv

Ter ondersteuning zijn conditionele voorwaarden en domeintabellen opgenomen in het GWSW-Hyd. In de .CSV bestanden is er ruimte voor het opnemen van meta-informatie van objecten, toelichting per record en aannames ten behoeve van de modellering. Het GWSW.hydx bevat een uitgebreide set mogelijkheden om gegevens ten behoeve van het uitvoeren van
hydraulische berekeningen uit te wisselen. Het formaat is flexibel zodat het mogelijk is om nieuwe definities toe te voegen en domeintabellen uit te breiden. Dit maakt het formaat toekomstbestendig.

## Netwerk
In het GWSW-Hyd wordt uitgegaan van een netwerkconcept, waarin onderlinge relaties en definities staan beschreven. Een netwerk bestaat uit knooppunten en verbindingen, die allen zijn voorzien van een unieke ID (UNI_IDE). In KNOOPPUNT.CSV staat de fysieke ligging en kenmerken van knooppunten beschreven. In VERBINDING.CSV staat
beschreven welke knooppunten op welke manier zijn verbonden en welke condities daaraan zijn verbonden. Meervoudige verbindingen krijgen per verbinding een unieke ID. Knooppunten en verbindingen hebben unieke eigenschappen die in andere .CSV bestanden worden toegekend aan de unieke ID (zie het [Voorbeeld Knooppunt](#knooppunt) of [Voorbeeld Verbinding](#verbinding) )

## Kunstwerken
Een kunstwerk kan een knooppunt of een verbinding zijn. Binnen het GWSW-Hyd wordt voorgesorteerd op de meest gangbare kunstwerken in de riolering, te weten pomp, overstortdrempel, doorlaat en uitlaat. De domeintabellen bieden ruimte voor uitbreidingen hierop. In KNOOPPUNT.CSV wordt in het veld KNP_TYP aangegeven als het om een
uitlaat gaat. In VERBINDING.CSV wordt in het veld VRB_TYP aangegeven om welk kunstwerk (pomp, overstort, doorlaat) het gaat. Een gemaalpomp, een doorlaat en een overstortdrempel zijn altijd een verbinding. Een uitlaat is altijd een knooppunt.

In KUNSTWERK.CSV wordt per unieke ID in het veld KWK_TYP opgenomen welk type kunstwerk het is. Vervolgens moet er per type kunstwerk een aantal verplichte waarden worden gedefinieerd (zie de [Voorbeelden bij Kunstwerken](#kunstwerken) ). Zo heeft een pomp o.a. een aan- en afslagpeil en heeft een overstortdrempel o.a. een drempelhoogte en -breedte. In KUNSTWERK.CSV kunnen ook specifieke Qh-relaties worden toegekend. Dit gebeurt in de velden QDH_NIV en QDH_DEB, waar de waarden getabuleerd kunnen worden opgegeven, met spatie als separator.

Met het netwerkconcept is het mogelijk om een compartimenteerde put op te nemen in het uitwisselformaat. Ieder compartiment krijgt een unieke ID, en daardoor ook eigen kenmerken toegekend. De verbinding tussen de compartimenten wordt ook gedefinieerd (zie het [Voorbeeld bij Gecompartimenteerde put](#compartiment) ).

## Belasting
De belasting op een rioleringsstelsel heeft een neerslagcomponent (RWA) en afvalwatercomponent (DWA). Dit kan als directe belasting of als indirecte belasting (ook wel lateraal debiet genoemd) aan de schematisatie worden toegekend. 

### Neerslag
De directe neerslagcomponent wordt bepaald door het aangesloten oppervlak dat is opgenomen in OPPERVLAK.CSV gekoppeld aan een neerslag-afvoerconcept. In GWSW.hydx is het NWRW-inloopmodel als 0D neerslag-afvoerconcept standaard opgenomen. Andere concepten kunnen in de toekomst worden toegevoegd, maar zullen vermoedelijk in het
2D-terreinmodel worden opgelost. In OPPERVLAK.CSV wordt per unieke ID (knooppunt of verbinding) aangegeven hoeveel vierkante meter (AFV_OPP) er via welk concept (AFV_DEF) met welke kenmerken (AFV_IDE) op afstroomt. Per unieke ID kunnen meerdere neerslagafvoerconcepten en specifieke kenmerken worden opgenomen door meerdere records van dat unieke ID op te nemen
(zie het [Voorbeeld bij Oppervlak](#oppervlak) )

## Vuilwater en lateraal debiet
De afvalwatercomponent wordt bepaald door belasting op een unieke ID (knooppunten en verbindingen) vanuit (directe) pandaansluitingen en (indirecte) laterale debieten (Paragraaf 4.5). In DEBIET.CSV wordt per unieke ID in het veld DEB_TYP aangegeven wat het debiettype is. Dit kan alleen vuilwater zijn (VWD) of een lateraal debiet met
vuilwater en/of oppervlak (LAT). Denk bij dat laatste aan inprikkers van drukriolering, de inprik van een ‘bakjesmodel’ of een proceswaterlozing.

Als het alleen vuilwater is of een lateraal debiet met vuilwater, dan moet in DEBIET.CSV het aantal vervuilingseenheden (AVV_ENH) worden opgegeven. Als het een lateraal debiet is met verhard oppervlak, dan moet in DEBIET.CSV het afvoerend oppervlak worden opgegeven (in veld AFV_OPP). Bij een lozing van bijvoorbeeld proceswater hoeft het aantal vervuilingseenheden en het afvoerend oppervlak niet te worden opgegeven.

In het veld VER_IDE van DEBIET.CSV wordt verwezen naar de verloopdefinitie die in VERLOOP.CSV is opgenomen. Het GWSW.hydx biedt mogelijkheden voor constant en variabel verloop over de week en over de dag. Per unieke verloopdefinitie (VER_IDE) moet er in VERLOOP.CSV één record worden opgenomen. In het veld VER_TYP wordt het verlooptype aangegeven met de opties:
- DAG – Variabel debiet per uur met vast dagvolume
- CST – Constant debiet per uur met variabel dagvolume
- VAR – Variabel debiet per uur met variabel dagvolume

## Meta-informatie
Het bestand META.CSV bevat alle relevante meta-informatie behorende bij het uit te wisselen GWSW.hydx. Denk hierbij aan het aantal .CSV bestanden dat bij het GWSW.hydx behoort, zodat de volledigheid te controleren is, maar ook aan de vermelding van de opdrachtgever, de uitvoerende, enzovoort

## Extra ondersteunende definities
Een verbinding kan een leiding of kunstwerk zijn. Dit wordt gedefinieerd in het veld VRB_TYP. Afhankelijk hiervan kunnen er losse profieldefinities (in PROFIEL.CSV voor VRB_TYP=GSL, OPL, ITR of DRL) en vervolgens eventueel ook definities voor infiltratie (in ITOBJECT.CSV voor VRB_TYP=ITR) worden opgegeven.

In PROFIEL.CSV wordt in veld PRO_VRM gedefinieerd welke vorm het profiel heeft. Dit kunnen vaste vormen zijn, zoals rechthoekig of rond. Dit kunnen ook afwijkende vormen zijn. Voor profielen in open water dient gekozen te worden voor XY-profielen (XYP). Voor gesloten profielen dient gekozen te worden voor ‘tabulated’ profielen (TAB). In de bijbehorende waardevelden (TAB_BRE en TAB_HGT) worden de waarden opgegeven met een spatie als separator.

# Voorbeelden
<a id="knooppunt"></a>

## Knooppunt 

<a id="verbinding"></a>
## Verbinding

pm

<a id="kunstwerken"></a>
## Kunstwerken

pm

### Gemaal

pm

### Overstort

pm

### Doorlaat

pm

## Bijzondere constructies
<a id="compartiment"></a>
### Gecompartimenteerde put
pm

<a id="oppervlak"></a>
## Oppervlak
pm

## Vuilwater en lateraal debiet
sd

## Profiel
sdf

# Didactisch Stelsel

## Schema Didactisch Stelsel 

Presentatie met QGIS op basis van de voorbeeld-dataset op https://apps.gwsw.nl/doc/GwswDataset__DidacStelsel_v1.6.orox.ttl

<img src="media/Schema didactisch stelsel.png" style="width:100%;height:100%" />

pm

# HydX-download

Dit hoofdstuk beschrijft de werking van de HydX-download met GWSW-Apps.

De basis voor deze applicaties vormen queries op https://github.com/StichtingRIONED/gwsw_queries/blob/main/apps/Hyd . 
Deze queries lezen de relevante gegevens uit de GWSW-Datasets. 
Om deze queries te kunnen lezen is enige kennis van SPARQL nodig maar inzicht in de werking van deze queries is 
wel noodzakelijk om de werking van de HydX-download in de basis te begrijpen. 
Dit document licht alleen de aanvullende bewerkingen van de query-resultaten toe.

## Gegevens knooppunten
Zie ook de query op https://github.com/StichtingRIONED/gwsw_queries/blob/main/apps/Hyd/Hyd_Knooppunt.rq 

### Gegevens compartimenten
Compartimenten worden als knooppunten meegenomen, voor elk compartiment in de dataset zijn gegevens zoals afmetingen nodig.

De volgende gegevens van compartimenten worden zo nodig wel overgenomen van het compositie-object (de put of het bouwwerk):
- Materiaal
- Maaiveldschematisering
- Bergend oppervlak
- Hoogte (vanaf versie 1.6 bestaat het kenmerk HoogteCompartiment, dat gegeven wordt bij voorkeur gebruikt)

Het bergend oppervlak werd in eerdere versies zonder bewerking overgenomen van het compositie-object. 
Vanaf 20240814 wordt het bergend oppervlak evenredig verdeeld over het aantal compartimenten in het compositie-object.

### Gegevens hulpstukken
In het netwerk kunnen aansluitpunten en hulpstukken als knooppunt voorkomen. 
Denk dan bijvoorbeeld aan uitlaatpunten, verbindingsstukken (bochtstukken, verloopstukken) of afsluitstukken.
In het HydX-formaat is voor deze knooppunten niet een apart type gereserveerd, daarom worden hulpstukken in de HydX-download gedefinieerd 
als een geknevelde Inspectieput (code INS) met minimale afmetingen (1x1x1 mm).

### Niveau binnenonderkant
Het HydX bevat in kolom KNP_BOK het niveau van de binnenonderkant knooppunt. Deze waarde wordt als volgt afgeleid:
- Als de punt-geometrie 3 dimensies heeft: de Z-waarde uit de geometrie
- Anders, als het knooppunt een put, bouwwerk of compartiment is: het gekoppelde maaiveldniveau verminderd met de knooppunt-hoogte

### Omgaan met blinde en verdekte put
De GWSW-types BlindePut en VerdektePut zijn in de dataset altijd extra (uitvoerings)typeringen. 
Daarnaast is altijd het functionele type (Inspectieput, Overstortput, ...) in de dataset aanwezig. 
De blinde put wordt wel meegenomen in de queries, zie ook https://github.com/StichtingRIONED/gwsw_queries/blob/main/apps/Hyd/Hyd_Knooppunt.rq . 
Het type BlindePut heeft effect op de maaiveldschematisering, die wordt "Gekneveld". 
Dat geldt niet voor verdekte putten omdat de mate (massa) van verdekking onbekend is.

## Gegevens verbindingen

Zie ook de query op https://github.com/StichtingRIONED/gwsw_queries/blob/main/apps/Hyd/Hyd_Leiding.rq 

Het **type Inzameling** wordt op basis van het leidingtype afgeleid:

| URI Leidingtype            | Code | Opmerking                        |
|----------------------------|------|----------------------------------|
| GemengdRiool               | GMD  |                                  |
| Overstortleiding           | HWA  |                                  |
| Vuilwaterriool             | DWA  |                                  |
| Hemelwaterriool            | HWA  |                                  |
| Infiltratieriool           | HWA  | (20190430)                       |
| VrijvervalTransportleiding | NVT  | Gebruik het supertype (20190430) |
| OpenLeiding                | NVT  | Gebruik het supertype (20190430) |

Het **type Verbinding** wordt op basis van het leidingtype afgeleid:

| URI Leidingtype            | Code | Opmerking                        |
|----------------------------|------|----------------------------------|
| GemengdRiool               | GSL  |                                  |
| Overstortleiding           | GSL  |                                  |
| Vuilwaterriool             | GSL  |                                  |
| Hemelwaterriool            | GSL  |                                  |
| Infiltratieriool           | ITR  | (20190430)                       |
| DIT_riool                  | ITR  | (20240816)                       |
| VrijvervalTransportleiding | GSL  | Gebruik het supertype (20190430) |
| OpenLeiding                | OPL  | Gebruik het supertype (20190430) |

