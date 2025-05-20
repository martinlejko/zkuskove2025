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
