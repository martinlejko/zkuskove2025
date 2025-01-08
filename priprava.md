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

Binary vs Text-based
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
    * gr:QuantitativeValue || gr:QualitativeValue

Slovnik Wikidata
    * strukturovana databaza
    * nejedna sa o texty ale skor o fakta
    * DBpedia/Wikidata qearable with SPARQL
    * Mame Q a P cisla ktore sa pouzivaju na mapovanie na predikaty a tak dalej cize niektore query niesu citelne
    * qualifiers upresnuju napr nie sestra ale mladsia sestra
    
## Prednaska 5.

LPG sa nehodi pro propojene data
RDF a jeho serializa sa hodi pre grafove data, kazdy uzel a hrana ma svoje URI

### LPG (Label Property Graph)
  * Nehodi sa na priemery a vypocty na to su lepsie tabulky
  * Skor sa hodi na relacie medzi vecami infrastruktura mesta
  * **pokial connections between entities are as important or more important then entities themselves** pokial riesime viacej nez 5 joinov lepsie asi graf
  * self-referencing management hierarchies alebo do hlbky priklad so zlatom a cipami

Vrcholy a hrany niesu reprezentovane IRI a hrany maju meno a moze to byt multygraf
Vrcholy mozu mat zoznam attributov a hrany mozu mat tiez attributy
Nemame ziaden structure takze mozeme mat ine attributy pri inych vrcholoch
Vrcholy od nazvu **labeled** maju oznacenie lablom napr vrchol :Person vrchol moze mat viacej lablov

Suhrn:
   1. Oriented multigraf
   2. Nodes have set of labels
   3. Edges have labels
   4. Nodes and Edges ahve sets of key-value properties

### Cypher
   * Query language for LPG
   * CREATE (:Person {name: 'John'}) -[:Follows]->(:Person {name: 'Jozo'})

Pouzivanie MATCH noodle Return variable ako select. napr MATCH (p:Person)-[:Owns]->(c:Camera) WHERE p.name = 'James' return c
MERGE funguje pokial uz je v zozname veci tak nic nerobi inac spravi CREATE

Dobry usecase je najkratsia cesta v grafe to vieme dostat cez: 
MATCH (Kevin:Person {name: 'Kevin Baco'}), (Alpa:Person {name: 'Al Pacino'}), p = ShortestPath((KevinB)-[:ACTED_IN*]-(Al) RETURN p

NULL nemame skor je to not bound a taktiez regularne vyrazy cez =~ 'M.*'
Friend of a friend vieme spravit cez MATCH (p:Person {name: 'Vit'}) --(other-person)-->() WITH other-person, count(*) AS foaf WHERE foaf > 1 RETURN other-person.name

Mozeme nacitat cez CSV files pomocou LOADCSV WITH HEADERS FROM 'file:///orders.csv' AS row
Podporuje to viacero grafovych algos

### GQL Standard
pracuje sa na Standarde pre querovanie LPG

### Srovnanie RDF A LPH
   * RDF je made for web and distribued data, globally reused RDF vocabularies, IRIs for everything. focused on linking data from various publishers
   * LPG made for centralized graph data, local node lables and edge types. every database instance uses different relationship types and node lables, better for graph algos

### RDF* (rdf star)
rozsirenie RDF pricom pridava quoted triple kde potom mozeme pouzit << _a: :name "Alice">> :statedBy :Bob .
Je to based na **RDF reification** ale slabsie kde budeme mat nieco ako:
```
_:stmt1 <rdf:type> <rdf:Statement> .
_:stmt1 <rdf:subject> <John> .
_:stmt1 <rdf:predicate> <hasAge> .
_:stmt1 <rdf:object> "30" .
```

## Prednaska 6.

### XML
   * Smer vazby urcuje ako nasa xml schema bude vyzerat niekedy potrebujeme viacero schem aby sme boli schopny popisat cely graf
   * XML v 1.0 sa pouziva Unicode 2.0
   * XMLS is **well-formed** if it complies with XML syntax rules
   * it is NOT good to represent tabular data
   * Pouziva sa na konkretne formaty napr SVG
   * Parsujeme cez DOM SAX StAX LINQ
   * Validujeme cez DTD Schematron alebo XML Schema
   * Querying cez XPath alebo XQuery
   * Transofmation cez XSLT
   * Dobre sa cez to posielaju message cez internety
   * XML is valid if it validates agains an XML schema for instance XML Schema or DTD

### Qualified name (QName)
   * uniquely identify elements, typically in the context of namespace
   * vychodzi namespace je ak to definujeme v <root xmlns="http://example.orf"> potom ostatne v xml spadaju pod tento namespace ak nieje specifikovane inak
     
```
<book xmlns:fiction="http://www.example.com/fiction" xmlns:science="http://www.example.com/science">
<fiction:title>The Great Novel</fiction:title>
```

CDATA section --> <![CDATA[<greetings> Heloo !!! ]]> treated as a string
Pre language specification pouzijeme language tag --> xml:lang="de"

XML processing instruction (PI) cez <?xml-stylesheet type="text/xsl" href="media-types.xsl"?>

process of FLATTENING when we take something hierarchical and try to fit it into table exmaple: types of coffe and when he drinks it

### How can we process XML data in an application?
   1. Document object model (DOM)
      - does not work for streams
      - does not work for large files
      + supports arbitrary querying (aka random access) eg XPath / consumers/consumer[1]/name
   2. Simple API for XML (SAX)
      + works for streams
      + works for large fies
      - does not support effective arbitrary querying
      SAX parser pushes events to your code until it reads the whole file
   3. Streaming API for XML (StAX)
      + works for streams
      + works for large data
      - does not support effective arbitrary querying
        our code pulls events from the StAX parser, so we can

### Serialization of XML
XML Syntax
   * z RDF do XML
   * pozriet si to este trochu viacej
     
### Formats where XML is used
 SVG 
 ```
<svg height="210" width="600">
   <polygon points="200.10 250.190 160.210"
style="fill:lime;stroke:putple" />
</svg>
```
OOXML - open office xml format
RSS alebo Atom sluzici pre Web Feeds

### Validation with XML Schema
   * we can link to XML Schema without using a namespace with XSD like to xsi:noNamespaceSchemaLocation="schema2.xsd">
   * napr editentiat.cz to endcoduje base64 xml trvaleho bydliska
   * Basic Principles
        - Definitions of Data Types Elements or Attributes
   * Simple type is needed when it does not have sub elements or attributes
   * mozeme pouzit simple type restriction

ak to ma iba attributy ale nie subelements wi use simpleContent napr na restrictions a extension opakom je ComplexType ak chceme sibelements

Complex Type - squence & element order dava pozor ako to mame za sebou poukladane elementy a ci su za sebou spravne 
Complex Type - choice je valid ak je iba jedno z nejakuch 
Complex Type - all musia byt vsetky poradie nezalezi

cez ref sa odkazujem na uz zadeklarovany element

ak nase XML pouziva nejaku schemu (XSD) tak potrebujeme to zadefinovat v XML cez xsi:schemaLocation

## Prednaska 7.
Hierarchical data formats XPath and XSLT
### XPath
   * Query language on XMLs
   * napr /catalog/datasets/dataset/title
   * mozeme pouzit aj relative path a nezaciname cez lomitko ale rovno title[@xml:lang="en"]/text()
   * predicate --> /catalog/datasets/dataset/title[@xml:lang="en"]/text()
Pohibujeme sa cez osy a zahladnou je child ale default vieme omitnut  --> axis::node-test [predicate1]...[predicateN]
napr /child::catalog/child::datasets

v child ose su iba elementy cize vieme spravit catalog/child::*

Axes - descendant - potomky

/catalog/descendant::title --> je jedno ako hlboko no zamerujeme sa na elemnt s nazvom ak spravime /catalog/descendant::* tak nam to vrati vsetky pod elementy catalogu

Axes -attribute ma skratku /@ cize ak chceme vsetky attributy catalogu a jeho podelementov tak spravime /catalog/descendant::*/@* ale taktiez vieme napisat /catalog/descendat::*/attribute::*

Axes - preceding-sibling - predchodzi surodenec
Axes - descendant-or-self mozeme pouzit ak chcem vsetky titly je to jedno ake zanorenie tak /catalog/descendant-or-self::title

Axes - self/parent


VSETKY AXES SU --> ancestor | preceding-sibling | self | following-sibling | parent | child | attribute | namespace | preceding | following | descendant

okrem os vieme pouzit funkcie a to je:
   * name()
   * position()
   * last()

document order je ako priradzujeme cislo element a ideme pocitat znacky od hora po spodok
XML Infoset
   * set of definitions for referring to infomation in well-formed XML document
   * created with no particular processing language in mind

### XSLT
   * ak chceme HTML reprezentaci dokumentu z XML
   * input is multiple or single XML and the output is one or more HTML RDF Turtle or TXT
   * modes umoznuju spracovat rovnake nodes ale different ways
   * a potom mame este switch alebo for each 
   XSLT stylesheet
      * is an XML document
      * stylesheet root element
      * set of templates
     * pouziva to XPath match na to aby sme vylnili veci a potom xsl:value-of select="co chceme"
   XSLT template
      * matches part of input XML document using XPath expressions
      * produces output text
   XSLT processor
      * goes through an input XML document
      * tries to match templates


Purpose of <xsl:apply-templates>
The <xsl:apply-templates> element instructs the XSLT processor to:
   * Select nodes in the XML document based on the current context.
   * Apply templates to those selected nodes, based on matching patterns defined elsewhere in the XSLT stylesheet.

Prednaska 8.

### JSON
   * Je to string kde pouzivame utf-8 a potom \escapes kde mozeme pouzit \u4hex number
   * No octal or hexadecimal digits, decimal only
   * Najviac sa to pouziva pre API alebo pre ofn.gov
   * jq ako query tool z command line
   * JSON Pointer aby sme mali iba nejake values from JSON nieco ako XPath 

### JSON Schema
   * motivacia je annotace a taktiez validacia co tam ma nemusi byt atd
   * nieje to standard still under development
   * type nam hovori co za typ riesime napr object
   * properties urcuju co tam moze byt v tom objekte a potom ich napr type alebo description rozne properties mozeme ma numerical constrains maximum minimum atd
   * required je list poloziek ktore tam su povinne
   * list validation cez napr maxItems contains uniqueItems pre urcity property
   * tuple validation kde kazdy jeden v poly vieme validovat aky ma typ cez "items" : [{"type": "number"} ...., {"type": "string", "enum": ["street", "avenue"]}]
   * cez aditional items vieme napr povedat ze iba stringy mozu byt additional

   * podpora pre external schema aby sme nemuseli mat brutalne velke fily kde si vieme spravit "$ref": "https://definicia-schemy.json""

   * ak chceme validovat nieco vacej ako zakladne typy pouzivame formats to nam umoznuje validovat --> iri uri date-time email a tak dalej

### JSON-LD
    * json for linked data
    * do obycajneho jsnu pridame JSON-LD @context a spravime JSON interpretable as RDF model pomocou mappingov by @keywords
    * pomocou @id vieme priradit konkretne IRI
    * @type pridanie IRI classy ktoreho typu je vieme do @context pridat mapovanie ako je "Restaurant": "http://schema.org/Restaurant" a potom v @type: pouzijeme rovno iba Restaurant
    * relativne IRI cez @base: "http://exmaple.org"
    * compact IRIs cez "foaf": "http://example.ordf/foaf" a potom niekde foaf:name : "Dave"
    * scoped @context pre rozne zanorenia
    * @language pre language tagy
    * mozeme pouzit @reverse na to kedze mame parent ale nie child tak to reversneme cez @reverse : "http://vocab.org/#parent"
    * aliasing keywords znamena vztahy medzi napr url : "@id" ak niekomu vadia zavynace tak si to vieme premapovat na slova
    * taktiez vieme pridat external context ako jsonld

## Prednaska 9.
### Relational data models -- CSV
    * PRED csv BOL dsv COMES FROM unix and separator is the main thing in ICO it is a pipe
    * TSV tab separated tabulka a odellovace su taby
    * csv nieje dobre nacitavat po riadkoch kedze existuju " a "" cize to moze jeden riadok byt roztiahnuty na viacero
    * rovnake datove typy ako xml a to cez xsd:date 
    * redundance je ked nieco nieje zavysle na tom druhoom cize vieme rozdelit to viacerych csv filov
    * vieme cez URL vybrat iba napr riadok a stlpec alebo cell cez link/link#col-2 alebo #row-3 #cell-4,1 ak nepodporuje to RFC tak to da cele

### CSV on Web
    * List of standards and recommendations (primar and a set of 4 w3c recommendations
    * Model for tabular data / vocabulary RDF for tabular data / generating a JSON from tabular data / generating RDF from tabular data
    * JSON-LD describtor for CSV helps with annotation validation and trasnforming to different formats
    * znova pouzivame @context kde definujeme @base alebo @lang the base is for metadata not data
    * @id je tableGroup pre viacere tabulky v JSON-LD descriptor
    * titles mozeme mat s viacerymi nazvami pre ine jazyky alebo list slov 
    * podpora pre primaryKey a foreignKey tak isto aj so sechamatami
    * CSVW podporuje formu dialectu kde to bude spracovavat aj zle CVS po nastaveni dialektu
    * pri vytvarani RDF spravime blank node a pridame 
    * virtual collumn is used for adding data when transforming CSV

Transformations - we have url of the script tempalte we have script format output format
Taktiez mame podporu pre notes a obojstranne tablky

pred CSVW bolo frictionlesstable.io mame JSON descriptor

## Prednaska 10.

