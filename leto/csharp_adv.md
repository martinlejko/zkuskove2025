# 1. Prednaska
Implicitni konverze (dava to zmisel ak string dedi on objectu dava zmysel ze object o = string) --> vlastne obe su referencne typy, cize sa vimenia iba adresy a zuzi sa pocet funkcii ktore mozeme volat.
    * tu vieme ak nieco on niekoho deni a naopak alebo pri val typoch davame mensi do vacieho alebo boxing
Explicitna konverzia je ak do string s = object tak az za runtime vieme co ten objekt je ak to je string fajn ak nie tak vynimka coz nieje dobre

* Extention methody -- syntax sugar ale nemusime specifikovat z akej clasy ta methoda je to sa prelozi v CIL kode, extention methody musia byt jednoznacne ak mame 2 rovnake z inych class tak sa to neprelozi
    * trieda v ktorej extention methody mame musia byt staticke aby sa lahsie vyhladavali a vyhladava v danom namespace (staticka znamena ze nevytvarame instance ani promnenne z tychto tried)
    * extention methody nikdy nemenia tu hlavnu funkciu na danom type az ked ta methoda neexistuje az potom sa zacina hladat extension methoda

# 2. Prednaska
* method overloading - methody co sa volaju rovnako ale su odlisne - v CIL kode sa to zvycajne oznacuje apostrofom
* robi sa pomocou --> *static operator expl/implicint A(B b)* kde to znamena cestu user defined z B do A
    * konkretna methoda sa vybera hned pri generacii CIL kodu
    * methody sa vyberaju v danem contextu a dle arity - najspeficikejsi co najviac pasujuci + nejmene prace
        * cize ak by sme mali dve methody f(object) a f(valueType) a davali by sme do nej int tak pri oboch dochadza k boxu ale valueType je blizsie menej prace preto sa vyberie on
        * imlicitni konverze je menej prace nez boxovanie ale vyziva sa MAX 1 UZIVATELSKA koverzia
    * ak sa methoda nenajde tak sa presunieme do vyssieho kontextu
    * rozdiel medzi volanim this A ak mame B : A a base je to ze base vola non-virt a this si vybera podla najlepsieho mozneho
    * classy sa beru za separatny kontext

* Genericke methody
    * CIL kod ma koncept genericke methody narozdiel od C++
    * cize implementacia methody odstane genericka ale v CIL kode ak sa vola tato methoda tak sa tam doplna typ --> CALL Max<int>
        * ked sa dana methoda vola pozrieme sa ci JIT uz pre dany typ vygeneroval kod ak nie vygeneruje sa
    * ak pouzivame overload methody v generickej methode tak nedochadza k meneniu sa ale vyzera sa stale ta najgenerickejsia

# 3. Prednaska
* Compile time duck typing pre genericke methody v C++
* Dynamic --> object + python ducktyping ON size mali by sme code nieco ako *void PostPackage(dynamic carrier){}*

* rozdiel preco f<T>(T t) where T:I1 a f(I1 t) niesu rovnake tak pre f(I1) sa vygeneruje iba jedna varianta v CIL kode no pre genercike sa sice musi vygenerovat viac no vie sa to lepsie optimalizovat a taktiez nieje tam overhead boxingu ak by sme davali napr int co je value type to ref typu
* Ale robit to genericky iba ak viem ze sa tam budu vela volat value typy
* where podmienka pri generickych methodach pouziva AND cize f<T>(T t) where T:I1 AND I2 AND I3
* genericke typy --> ak by sme mali Class X<T> tak sa to v CIL kode prelozi iba raz a keby ze to chceme niekde pouzit tak musime explicitne povedat s akym typom
    * ak pride na to ze sa pouziva X<string> alebo X<int> tak sa vygeneruje novy CIL kod a tieto typy nemaju spou nic spolocne ani rodica
    * strojovy kod sa JITuje iba ak je nutno napriklad ak by mala X<T> methodu m() a inicializujeme X<int> tak X<int>.m() sa neJITuje
* su to zcela nezavysle typy a nemaju spolu nic spolocne!!
* mozeme mat genericku methodu v generickej methode

# 4. Prednaska
* Explicitna deklaracia method interfacu - priklad s writerom a readerom

```
interface I1{
public void m1();
public void m2();
}

class A : I1{
public void m1(){};
       void m2(){};
}
```
zaruci ze nemozeme volat methodu m2 na acku ani vo vnutri ani z vonku ide ju zavoat iba ak sa na A budeme pozerat iba cez I1.
* pri dedicnosti musime ale ukazovat presne ktoru methodu myslime

* alfa --> Ak B je kompatibilny s A
* Type C je parametrizovanym typom T --> C<T> a zaroven C<\B> je typovo komparibilne s C<\A> 
    * potom C je covariant podle T
* pokial alfa a Typ C je parametrizovany typom T a zaroven C<\A> je typovo kompatibilne s C<\B>
    * potom C je contravariant podle T
* ak nieje ani ani tak je invariantny

* v C# su vsetky genericke typy invariantne
* pole referencnych typu podle T su covariantne podla T

# 6. Prednaska
* namespaces --> ako funguju a ze je to iba zapisane ak je nieco v namespace A{} tak je to iba ako A.niecoVnom
* nested types
    * ak mame classu A v namespace X a v nej nested class B tak to v CLR vyzera ako X.A v nej nejaky kod a potom iba B v stacktrace budeme vidiet X.A + B no na urovni c# to ale bude mat podobny zaznam X.A.B
    * na nested classy mozeme aplikovat taktiez public, private a protected a tym lepsie nastavovat visibility
    * nested tym nemoze robit s nadrazenym ak by sme ho jej nepriradili (nieje to ako v jave cez this)
    * napriklad imlementacia sorted dictionary kde mame disctionary a private class Node aby sme nic ine nerozbili
    * vieme naimplementovat private interface pre class A a nested classa B bude splnat tento kontrakt potom vieme volat tuto metodu iba v classe a ak by sme sa pozerali na classu B cez tento private interface
    * Ak predame nested classe B classu A tak potom B vie sahat na private veci classy A

