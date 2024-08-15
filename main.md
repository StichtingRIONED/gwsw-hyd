# Ondersteuning in het gebruik van GWSW

<style>
  .symbolSmall{width:20px;height:20px;margin-right:1em;vertical-align:middle}
  .symbol{width:30px;height:30px;margin-right:1em;vertical-align:middle}
</style>

Vanuit Stichting RIONED is Eric Oosterom de verantwoordelijk projectmanager. Vragen over deze website en het GWSW kunt u stellen via gwsw@rioned.org. 

# Didactisch Stelsel

## Schema Didactisch Stelsel 

Presentatie met QGIS op basis van de voorbeeld-dataset op https://apps.gwsw.nl/doc/GwswDataset__DidacStelsel_v1.6.orox.ttl

<img src="media/Schema didactisch stelsel.png" style="width:100%;height:100%" />

pm

## Put

pm

## Leiding

pm

# Kunstwerken

pm

## Gemaal

pm

## Overstort

pm

## Doorlaat

pm

## Bijzondere constructies

pm

### Gecompartimenteerde put

pm

# HydX-download

Dit hoofdstuk beschrijft de werking van de HydX-download met GWSW-Apps.

De basis voor deze applicaties vormen queries op https://github.com/StichtingRIONED/gwsw_queries/blob/main/apps/Hyd . 
Deze queries lezen de relevante gegevens uit de GWSW-Datasets. 
Om deze queries te kunnen lezen is enige kennis van SPARQL noodzakelijk.
Inzicht in de werking van deze queries is noodzakelijk om de werking van de HydX-download in de basis te begrijpen. 
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
In het netwerk kunnen hulpstukken als knooppunt voorkomen. Denk dan bijvoorbeeld aan verbindingsstukken, bochtstukken en verloopstukken.
In het HydX-formaat is voor deze knooppunten niet een apart type gereserveerd, daarom worden hulpstukken in de HydX-download gedefinieerd 
als een geknevelde Inspectieput (code INS) met minimale afmetingen (1x1x1 mm).


