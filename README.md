# C++14 – Schnelleinführung für jemanden, der C# kennt. :-)

https://en.cppreference.com/w/

## Geschichte & Verwandtschaft zu C
https://en.cppreference.com/w/cpp/language/history
* K&R C → ISO C 90 → (C95) → **C99** (→ C11 → C18 …) 
* "C with clases" → ISO C++98 → (C++03) → **C++11** → C++14 (→ C++17 → C++20 … )
* ISO C, ISO C++, POSIX, …
* C: "Trust the programmer"
* C++: "You don't pay for what you don't need"
* Fluch und Segen: Kompatibilität (zu C und zu älteren C++-Versionen)

##  Übersetzungsschritte
https://en.cppreference.com/w/cpp/language/translation_phases
* de jure 9 Schritte/"Phases", de facto können sie zusammengefasst werden ("as-if" rule)
* Praxis, meist folgende 4 Schritte
  * "Präprozessor" (.hh+.cc → .ii) – Phases 1…4
  * Compiler ( → .s) – Phase 7
    * "translation unit"
    * Template-Instanziierungen (implizit, explizit) – Phase 8
  * Assembler (→ .o)
  * Linker (→ .so, .a, executable) – Phase 9
    * "linkage"
    * Link-time optimization (LTO)

## Begriffsbestimmungen
* identifier vs. names  – https://en.cppreference.com/w/cpp/language/identifiers#Names
  * Reserved identifier: _keywords_, `…__…`, `_X…`, `::_…_`
  * Nicht-ASCII-Zeichen sind möglich (nicht alle), aber unüblich
  * "Ungarische Notation" ist unüblich und i.A. misbilligt (mag aber in manchen Libs andere Konventionen geben)
* declaration vs. definition
  * One-definition rule – Ausnahmen ("inline") – https://en.cppreference.com/w/cpp/language/definition
* Sichtbarkeit, Name Lookup
  * https://en.cppreference.com/w/cpp/language/lookup
  * https://en.cppreference.com/w/cpp/language/adl
* object vs. function
  * Lebensdauer (storage duration)
* Value categories
  * rvalue, lvalue, prvalue, glvalue, xvalue
  * https://en.cppreference.com/w/cpp/language/value_category#prvalue
* Beobachtbares Verhalten – https://en.cppreference.com/w/cpp/language/as_if
  * Ausnahmen:
    * https://en.cppreference.com/w/cpp/language/copy_elision
    * …
* Implementation-defined behavior, unspecified behavior, undefined behavior
  * https://en.cppreference.com/w/cpp/language/ub
  * UB macht das _gesamte_ Programm ungültig!
* RAII
  * Zur sicheren Verwaltung _aller_ Ressourcen, nicht nur von Speicher
  * exception-safety
* Iteratorkonzept
  * Halboffene Intervalle
  * Iteratorkategorien https://en.cppreference.com/w/cpp/iterator

## Typen
https://en.cppreference.com/w/cpp/language/type

* incomplete types vs. complete types
* Pointer vs. Array
* komplexe Deklarationen: `char *(*(**foo[][8])())[];`  http://unixwiz.net/techtips/reading-cdecl.html
  * Lösung: `typedef`, `using` deklarationen
* Typ"umwandlungen":
  * implizit vs. explizit
  * werterhaltend vs. verlustbehaftet
  * vordefiniert (built-in) vs. benutzerdefiniert
  * `static_cast`, `dynamic_cast`, `const_cast`, `reinterpret_cast`, Function-Style casts und C-Style casts:  `int i=(int)3.14;`

## Benutzerdefinierte Typen
  * `struct`, `class`, `union`, `enum`
  * Member (data member, member functions, virtual)
  * Speicherlayout
    * Empty base optimization – https://en.cppreference.com/w/cpp/language/ebo
  * Konstruktoren, Destruktoren, Operatoren
    * Copy-C'tor, Move-C'tor, "Rule of 3", RAII, `explicit`
    * `=default`, `=delete`
  * Vererbung
    * Mehrfachvererbung, "virtual base class"
    * Abstrakte (Basis-)klassen
  * Pointer to member
    `int T::*pm = &T::x; T t; t.*pm = 42;`
