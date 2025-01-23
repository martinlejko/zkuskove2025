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
