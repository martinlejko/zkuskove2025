## Prednaska 1

* stack/heap pre Python, CPP a C#
* value and reference types a ako sa ukladaju na stack/heap
* co sa deje ked sa zavola new() pre referencne a hodnotove typy
* rozdiel medzi class a record

## Prednaska 2

* syntax sugar s var a taktiez s new()
* kedy pouzit struct a class (priklad s Class Color)
* properties a pouzivanie get/set rychlejsie ako funkcie (priklad s maxHealth a healthPercentage)
* preskakovanie volania konstruktoru sa nikdy neda
* required
* variable capture, ak v konstructore Class mame variable ktoru niekde pouzivame v classe tak sa pre nu vygeneruje constructor
* pre record sa vygeneruju properties automaticky pre vsetky attributy a su (get; init;)
* record struct taktiez vzniknu properties ale su R/W
* ak spravime readonly record struct tak sa to chova ako record class
* nullable value

## Prednaska 3

* Interfacy (ducktyping)
    * fieldy v interfacoch no-go ale za to properties s get/set ano
    * taktiez vsetko method like

* !! pozriet na to cviko je tam o Interface tables
* aby som zrusil warning na memberhiding pri inheritance napisem -> public *new* menoFunkcie
* pomocou is sa vieme spytat ci je to instancia niecoho konkretneho a podla toho sa zachovat napr. if (A is B) { ... }

## Prednaska 4

* {zdrojaky .cs + .csproj} --[sa odovzdaju build systemu .NET (.dll)]-->{executable CIL code (pre vsetky zariadenia)}--[CLR (common language runtime) + JIT (just in time compiler)]-->x64 | x86 
* to je dobre ze sme compatible aj na urovni binarky a nie len kodu
* na JIT sa vieme pozerat ako na Virtual Machine a CIL je assembly
* optimalizacie sa robia az ked vieme goal architekturu
* preto ze mame tuto pipelinu nemoze JIT stravit vela caju na optimalizaciu
* miesto JIT vieme pouzit Ahead Of Time --> iny druh optimalizacie
* k CIL kodu sa generuju aj metadata aby sme vedeli sharovat funkcie medzi .dll
* kedze stym pracuje aj JIT tak budu tam data o vsetko aj o privatnych a public funkciach ale ak si pozrieme to cez ILSpy tak poradie to nevie
* v CIL vidime methody private parametry aj komentare
* v metadatach nieje ako sme si pomenovali premenne su dostatocne na pustenie ale nie na debuggovanie
* debuginfo .pdb preto to vedelo jak sa to vola ked to zmazeme a skompilujeme znova tak to meno nebude vediet

### Inheritance

* nededia sa iba instanstne veci ako su methody a fieldy ale aj staticke methody/fieldy aj ich pristup
* abstract class ak chceme aby vsetci to mali ale samu o sebe ju vytvarat nechceme (jedine co hovori je zakazuje vyrabanie instancii)
* C# robi pravidlo jedneho predka okrem typu object ktory predka nema, takze mame prave jedneho predka
* ale Interfacov vieme dedit 0..N
* konstruktory sa nikdy nededia moze viest ku nezmyslom na novych neinicializovanych fieldoch

## Prednaska 5

* class constructor --> iba 1 a vzdicky bez parametricky { definujeme tym ze napiseme static className }
* JIT vytvori ten constructor az ked je potreba \ lazy
* !! pozriet jak sa chova ked sa volaju construktory okolo 15-30 min prednasky
* nemozeme volat this aj base naraz lebo by vznikol rovnaky problem ako constructor predka
* ked sa stane chyba v konstructore tak .NET o tom hovori ako o metode .ctor
* static class hovori ze zakazujeme vyrabat instance a promnenne

Ako vieme zapisat constantnu hodnotu:
1. Ako read-only field
2. ako {get; init;}
3. static readonly field
4. static get only property
5. const {je strictne iba v metadatach assembly} a uz pri generovani CIL kodu sa pouzije tato hodnota | to je rozdiel medzi ostatnymi verziami | musi to byt ale compile time constant {znamena pri compilovani vieme aku hodnotu to nadobuda}

* sice optimalizacie robi az JIT no CIL robi constant folding co je ze niektore vyzrazy vie vypocitat a rovno pracovat uz snimi
* constanta je per program run a moze naberat iba jednoduche typy int, bool, double a string

* od .NET 8 sa zavadza uz constant folding aj do JIT

## Prednaska 6

* attributy ako je public static private tak ovplivnuju ako sa chova prekladac ked sa znich generuje CIL kod
* museli by to dat do metadat v dll a to je ovplivnuje aj ine jazyky v .NET
* custom attributes zapisujeme cez [ ... ] a poznamenava sa to potom v CIL
* napr [obsolete] kde nam to bude vytvarat warning
* je to iba popis, napr v CIL kode

* Enum a attribute [Flags] tak sa na to pozerame ako na bitovu masku
* knihovny cez nuget package .nupkg jednoduchy zip + metadata ktore potom pouzivame v kode .csproj
* JIT optimalizacied
* JIT intriasics vtedy ak vie ako sa ma ta methoda chovat tak vygeneruje
* nemali by sme robit micro-optimalizacie
* METHOD INLINING --> miesto volanie methody a nadbytocnych CALLov funkcie tak sa rovno do methody vlozi ten uzitocny kod

* tabulka virtualnych method

## Prednaska 7

* rozdiel vo volani virtual a non virtual method je ze pri normalnej sa vie spravit method inlining no pri virtiualnej sa robi CALL VIRT
* Dynamic PGO zbiera statisticky nie len ze kolko krat sa to vola ale co bolo za instancie pouzite
* napr hot path pre najviac volani kod a vie spravit de-virtualizaci to robi JIT
* sealed zakazuje rozsirovanie
* pozriet znova na tabulky

## Prednaska 8

* base pouzivame ked sa chceme pozriet na predka spatky a zavolat jeho methodu
* aj pre nevirtualne methody prekladac generuje CALL VIRT 
* aby JIT zistil s metadat to zoptimalizoval
* CALL methoda sa vygeneruje iba v pripadae ze volame base inac je to stale CALL VIRT az po optimalizaciu
* ak napiseme methodu do gettru a settru tak sa len vygeneruje funkcia ktora sa vola get_menoProperty a to iste aj pre set
* lambda => ako return statement a to ze nemusim pisat return a potom napr public int age => _oldAge robit to ze to ma iba getter nie setter musel by som spravit public int age { get => _oldAge, set => 10} ak chcem oba

* zivotnost promnennych --> nebat sa kratnych zivotnosti

## Prednaska 9

* nieje povolenne mat premenne s prekryvajucimi sa zivotnostiami 
* takziez to ma errory s nested blockami alebo outer blockami
* ify nepozerame ze jaky maju vysledok kedze by to nebolo deterministicke
* BOXING ked hodnotu value typu prirazdujeme do referenciho 
* robime to preto lebo si vieme spravit object O pre ktory spravime ToString a potom vieme zaboxovat don vsetko na co vieme potom volat tuto virtualnu methodu napriklad
* ducktyping pomocou params v konstructore funkcie kd potom nemusime pisat typ
* trunkacation z longu na int, nikdy sa nedeje unboxing a trunkacion naraz preto musime najprv spravit unboxing a potom casting

* tracking referencie --> nata na GC heap aleo field, moze to byt adresa ale neda sa zistit
* na cele fieldy mozeme iba nie napr na 2B od konca
* pomocou boxingu vieme dat hodnotovy typ na GC heap

## Prednaska 10

* call by refeerence mame pomocou ref int x
* referrencia je pointer na adresu
* out je nieco podobne no nevyzaduje inicializovanu premennu
* ale aspon daco sa musi zapisat za beh tejto methody do out variable iba hapy pathy moze skoncit throwom

* _ ako vyraz discard

## Prednaska 11

* Arrays je rozdiel medzi List
* Ak mame int[] a List<int> ak povieme pri liste a[0] tak je to iba syntax skratka za nejaky getter ale pri array pracujeme s hodnotou. Preto nemozeme pouzit tracking ref lebo to nereprezentuje nejaku hodnotu v poly ale 
* s goto mozem skakat iba vo vnutry funkcie nie z funkcie do funkcie a potom este zo zatvoriek
* potom switch case 

## Prednaska 12

* ked switch nema vetev kde ist tak sa vyhodi vynimka
* tracking referencie nie v structure a classe lebo ma to dlhzsiu zivotnost jak nieco co to vola
* ref struct je value type ktory moze mat tracking ref ale potom tu celu strukturu mozeme pouzivat iba tam kde tracking ref
* Span<T> je readonly ref struktura ( bezpecny pohlad do pamati ) s global indexerom na jednotlive objekty
* bezpecne pretoze mame dlzku cize nepozeme inde na pamat
* this[index] ma range check az ked span ref sa zrusi tak GC heap to vie zmazat
* span je stale na stacku a nemoze byt na heape
* potom existuje ReadOnlySpan

* advance featura stackalloc --> ma zivotnost do konca funkcie

* int[] a = [1,2] je collection initializor
* collection initializator je compatible so Spanom cize mozeme spravit Span<int> l = [1,2,3] co spravi optimalizaciu a nebude to allocovat na heape ale iba na stacku
* spread up operator vieme spravit cez ... kde ak mame int[] l = {1,2,3} tak vieme zapisat { -1 , ... l, -2, ... l } 
* ak nevie ci je stack dostatocne velky tak sa to naspat da na pole inac sa pouzije stack alloc je to tym ze nechce aby sa vyhodila stack overlow exception
* preto treba sa zamyslat ked pouzivame tie collection inicializator kedze nevieme co sa z toho pouzije alebo aspon musime nad tym rozmyslat

* pattern matching na array a so spread operatorom if (a is [>2,< 1, ... int[] restArray, int x]) ---> kde restArray je copia a vieme ju menit a vypisovat ako copiu listu
* to je pre list a array ale pre Span je to iba pohlad cize nie copia
* cize zmeni sa to povodne

* methoda deconstruct .deconstr kde ma vystupne parametry 1..n
* tieto deconstructory sa dogenerovavaju k record classam
* pre normalne classy si vieme dopisat methody Deconstruct(out var x, out var y) kde povieme ze vieme sa dekonstructovat do nejakych veci

* Exceptions --> mame to mat optimalizovane ze sa vynimky nedeju a ked sa netrigruje tak dava minimalny overhead
* ked sa hitne catch musi sa generovat stacktrace a preto je to narocne

## Prednaska 14

*
