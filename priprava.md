# Priprava

## Prednaska 1

* Data a aplikacia by mali byt od seba dodelene
* Ako spravne spravit interface pre posielanie medzi applikaciami

Conceptualnym pohladom na data
* Co za veci zo sveta popisujeme
* Ako su prepojene
* Ake su ich vlastnosti

Vtedy ked sa bavime s niekym kto nieje ITak ale je domenovy expert

Data Models vs Data Formats vs Data Schemas
Data Models --> Dava nam nastroje napr vrcholy a hrany alebo tabulky  cez ktore vieme popisat nase data
Data Formats --> Reprezentacie dat cez nastroje kde ideme ulozit ich do suboru alebo nejak inac serializovat do suboru ktory vieme posielat
Data Scheme --> Popis a ohranicenie pre datove formaty aby data boli lepsie popisane a validatovane (Meta-formats) 

### Vlastnosti generickych datovych formatov
Open vs. closed
* Open --> verejne dostupna specificacia dostupne na webe a ziadne limitacie
* railML.org napr je closed kedze musime mat certifikaciu aby sme povedali ze snimi robime
machine readability
* Skor sa to viaze na jednu instanciu datoveho suboru nez formatu, a ci datovy format bol dobre pouzity

binaty vs text-based
* Binarne subory su definovane na bit by bit leveli znamena ze niesu txt files a su viewed by *HEX EDITORS*
* Text based encoded into 1s and 0s using characters also viewable by hex editors ale zvycajne su to charaktery na riadkach napr using ASCII
* New line representation CR / LF
* Vsade sa vacsniou pouziva UTF-8 kde 1-4 Byty kde emojis use 4 bytes a vacsina characterov pouzivaju 2B

BOM - Byte order mark 
* Magic number at the beginning of a text file
* Indicuje unicode encoding

### Standardizacia
* Aby sme vedeli ako si to posielat, kto spravil chybu, kto to zaplati
* Publikovanie dat standardizovane je viac cost effective


RFC vs Web Standart
* RFC je iba request vo vyvoji takze nemusi sa stat standardom
* Pri RFC zvycajne chceme pouzivat presne upresnenia napr MUST, SHALL NOT a su tam nejake requirement levels 

### Identificator
URI -- URL -- IRI -- URN
Uniform Resource Identifier -- Uniform Resource Locator -- Internationalized Resource Idetifier -- Uniform Recource Name

Rozdiel medzi URI a URL je ze URL nam da link este na lokaciu toho objektu na internete
IRI a URI tak nam IRI z casti pouziva UTF8 takze mozeme pouzit emoticoni a tak dalej

## Prednaska 2
Graph data representation, kde vieme popisat grafom nejake tvrdenia napr The catalog has title "my catalog". kde budeme mat 2 vrcholy The catalog a "my catalog". pricom je to spojene hranou has title

Miesto the catalog vieme pouzit IRI alebo URL na lepsiu identifikaciu
Vztah vieme taktiez popisat pomocou url

Ale kedze URL mozu byt dlhe vieme pouzit techniku PREFIXOVANIA kde budeme mat definicie a pouzijeme napr dctems:title, ex:catalog atd

Predosle tvrdenie potom vieme zapisat --> ex:catalog rdf:type dcat:Catalog

### RDF (graph based data model)
* Triple description --> subject predicate object
To jak to vyzera v txt suboru je uz serializacia mame napr RDF/XML, RDFa, N-Triples, Turle, JSON-LD, N-Quads, TriG

Mozeme mat a tiple with literal value alebo resource  

RDF serializations: IRIs and IRI prefixes
* <httpL//purl.org/dc/terms/creator> = @prefix dcterms: <httpL//purl.org/dc/terms/creator> . dcterms:creator

RDF je mnozina trojic takze nemame usporiadanie

mame este typed literals napr "2020-04-23"^^xsd:date alebo text literals with a language tag napr "Homepage of Jakub"@en

Mame nieco ako blank nodes to je _:a1 kde nieje to popisane ale viaze sa na to viacej veci preto ho pouzijeme 

### N-Triples
* jednoducha serializacia RDF kde vsetko zapisujeme pomocou IRI az na literaly co vieme zistit ak tam nieje IRI tak tam bude nejaky lieral ktory je nasledovany language tagom alebo IRI datoveho typu
* podporuje to komentare ale skoro nic ine preto je to jednoduche ale nic viacej neponuka
priklad: <http://one.example/subject1> <http://one.example/repdicate1> <http://one.example/object1> . #comments here

* Nevyhody su ze je to ukecane cize vela dat pri prenosu na webu
* Vyhoda da sa to ureznut velmi jednoducho kedze to je po trojiciach

### Trutle - prefixes and ";" and ","

To nam o dost skracuje text ak ma to viacej vlastnosti pouzivame ciarku ak je to koniec tak pozujime;
* Relativne IRI su relativne k necemu takze potrebujeme zadefinovat nieco ako @base a ak base nieje zadefinovana tak moze to byt URL suboru

rdf:type moze mat syntakticku skratku --> *a*
Ak pouzivame <a1> tak vieme povedat ze sa pouziva but absolutne alebo relativne IRI

* Statements about statements
--> napr: my:index.html my:createdBy "Jabuk klimek" .
Pouzijeme blank nodes na Reified statement.
_:triple1 a rdf:Statement
_:triple1 rdf:subject my:index.html
_:triple1 rdf:predicate my:createdBy
_:triple1 rdf:object "Jakub Klimek"
_:triple1 dcterms:created "2020-04-23"^^xsd:date

Toto bolo priamociare druhy sposob 

#### Reification by named graphs
Statements belong to "named graph"
RDF triples become RDF quads
RDF datasets consists of set of named graphs and default graph if we dont specify it fals under default

Cize budeme mat --> Subject Predicate Object Graph

### TRIG

rozsirenie RDF --> 1 default graph a N named graphs

ak nenapiseme GRAPH <IRI/graph> ale iba { ericFoaf:ericP :givenName "Rick" . } tak to spada do default graphu

### RDF Schema - Slovnik

definujeme clasy a hierarchie napr ex:Van rdf:type rdfs:Class. ale taktiez ex:Van rdfs:SubClassOf ex:MotorCycle

este mozeme pouzit rdfs:label/seeAlso/comment/isDefinedBy 

co ak chceme zachovat poradie? rdf:List je to ako spojovy seznam je uzavrety rdf:nill ma skratku a to ( :a :b :c)
mame tatiez rdf:Bag alebo rdf:Seq

Issues with regular not linked data is:
* Ambiguous identification of entitites in data
* low findability and accessibility of data
* no contextual information

To riesi World Wide Web kedze 

### Principles of Linked Data

1. Use URIs as names for things
2. Use HTTP URIs so that people can look up those names
3. When someone looks up URI provide useful information using the standards RDF or SPARQL
4. Provide links to other URIs so they can look up other things


**note owl:sameAs vieme pouzit ak mame viacej Subjektov rovnakych


## Prednaska 3.

5-star data --> Open machine readable URI odkazujuce sa na ostatne zdroje a este nejako otvoreny ??? to je LOD prepojene otvorene data

SPARQL --> query language pre RDF data
SPARQL endpoint
    * HHTP based web service
    * input query - output RDF/CSV

ak nieco neviem z RDF trojice tak to zapisujeme napr ?s dcterms:creator staff:8574 .
na vyplnenie niecoho ktore nevieme sa pouzije graph pattern matching

STRASNE VELA AKO QUEROVAT SPARQL

## Prednaska 4.

Slovnik *Dublin Core* --> need for standardizationa
    * ma 2 namespacy dc:prefix a rozsirenie dcterms:prefix 
    * zakladnych 15 pojmov
    * robili to knihovnici tak ma to dost podobny slovnik spojeny s knihami

Slovnik Simple Knowledge Organization System (SKOS)
    * popisuje vycty alebo hierarchie ciselnik znamek. tyo zvierat atd
    * skos:Concept prefix
    * aby to citelne nazvy tak pouzijeme skos:prefLabel to je preferovany mame taktiez skos:altLabel alebo skos:hiddenLabel
    * pre znamky vieme pouzit skos:notation
    * Skos podporuje aj semanticke relacie napr skos:related/broaderTransitive/semanticRelation/narrowerTransitive
    * alebo aj skos:Collections alebo skos:OrderredCollection kde pouzijeme skos:member < a>> <b> <c>  alebo ordered (<a> <b> <c>)
    * Mappings s exacMatch alebo iba Match

Slovnik GoodRelations (jedna sa eshopov a E-Commerce)
    * zaklad tvori gr:BusinessEntity gr:PorudctOrService gr:Offering a gr:Location
    * gr:OpeningHoursSpecification je linknute na jednotlive dni
    *  
