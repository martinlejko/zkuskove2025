# 1. Prednaska
Implicitni konverze (dava to zmisel ak string dedi on objectu dava zmysel ze object o = string) --> vlastne obe su referencne typy, cize sa vimenia iba adresy a zuzi sa pocet funkcii ktore mozeme volat.
    * tu vieme ak nieco on niekoho deni a naopak alebo pri val typoch davame mensi do vacieho alebo boxing
Explicitna konverzia je ak do string s = object tak az za runtime vieme co ten objekt je ak to je string fajn ak nie tak vynimka coz nieje dobre

* Extention methody -- syntax sugar ale nemusime specifikovat z akej clasy ta methoda je to sa prelozi v CIL kode, extention methody musia byt jednoznacne ak mame 2 rovnake z inych class tak sa to neprelozi
    * trieda v ktorej extention methody mame musia byt staticke aby sa lahsie vyhladavali a vyhladava v danom namespace (staticka znamena ze nevytvarame instance ani promnenne z tychto tried)
    * extention methody nikdy nemenia tu hlavnu funkciu na danom type az ked ta methoda neexistuje az potom sa zacina hladat extension methoda

