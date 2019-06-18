# C++14 – Schnelleinführung für jemanden, der C# kennt. :-)

https://en.cppreference.com/w/

## Geschichte & Verwandtschaft zu C
https://en.cppreference.com/w/cpp/language/history
* K&R C → ISO C 90 → (C95) → **C99** (→ C11 → C18 …) 
* "C with classes" → ISO C++98 → (C++03) → **C++11** → C++14 (→ C++17 → C++20 … )
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

* Exkurs: Wie werden C/C++-Programme üblicherweise gelinkt?

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

* Auswertungsreihenfolge (evaluation order, sequence order)
  * https://en.cppreference.com/w/cpp/language/eval_order
  * "sequence points" (vor C++11)
  * "sequenced-before" (seit C++11)
  * Regeln änder(te)n des öfteren. Nicht "zu schlau" coden!

* Implementation-defined behavior, unspecified behavior, undefined behavior
  * https://en.cppreference.com/w/cpp/language/ub
  * UB macht das _gesamte_ Programm ungültig!

* RAII
  * Zur sicheren Verwaltung _aller_ Ressourcen, nicht nur von Speicher
  * exception-safety

## Typen
https://en.cppreference.com/w/cpp/language/type

* incomplete types vs. complete types
* Nullzeiger
* Pointer vs. Array
* Pointer vs. References
* type qualifiers
  * `const`, `volatile`
  * const-pointer vs. pointer-to-const
  * `mutable`
  
* constexpr – https://en.cppreference.com/w/cpp/language/constexpr

* Zeichenliterale und Zeichenkettenliterale
  * `char`, `wchar_t`, `char16_t`, `char32_t`
  * "raw strings"
  
* Funktionen und Funktionszeiger

* komplexe Deklarationen: `char *(*(**foo[][8])())[];`  http://unixwiz.net/techtips/reading-cdecl.html
  * Lösung: `typedef`, `using` deklarationen

* Typ"umwandlungen":
  * implizit vs. explizit – https://en.cppreference.com/w/cpp/language/implicit_conversion
  * Promotion, Conversion
  * werterhaltend vs. verlustbehaftet
  * vordefiniert (built-in) vs. benutzerdefiniert
  * `static_cast`, `dynamic_cast`, `const_cast`, `reinterpret_cast`, Function-Style casts und C-Style casts:  `int i=(int)3.14;`
  * Umwandlungen von Zeigertypen
    * `char*` → `const char*` ✅
    * `char**` → `const char**` ❌  ☠️  🤔

## Benutzerdefinierte Typen
* `struct`, `class`, `union`, `enum`
* Member (data member, member functions, virtual member functions)
  * `const` Member, Referenzen als Member
* Speicherlayout
  * Empty base optimization – https://en.cppreference.com/w/cpp/language/ebo
* Konstruktoren, Destruktoren, Operatoren
  * Copy-C'tor, Move-C'tor, "Rule of 3", RAII, `explicit`
  * `=default`, `=delete`
  * `operator()`
  * Konvertierungsoperatoren
* Vererbung
  * Mehrfachvererbung, "virtual base class"
  * Abstrakte (Basis-)klassen
* Pointer to member
  `int T::*pm = &T::x; T t; t.*pm = 42;`


## Lambda-Ausdrücke
* catch clause
* return type
* generic Lambdas (seit C++14)

## Standardbibliothek

### Designkriterien & Konzepte
* Templates statt Vererbung ("Compilezeit-Polymophie" statt Laufzeit-Polymorphie)
* Iteratorkonzept
  * Halboffene Intervalle
  * Iteratorkategorien https://en.cppreference.com/w/cpp/iterator
  * Iterator-Adapter
    * Reverse-Iterator
    * Insert-Iterator
    * Move-Iterator

### Zeichenketten
* `std::basic_string<T>` T=`char`, `wchar_t`, `char16_t`, `char32_t` 
* kann NUL characters
* `"Foo"s` (seit C++14)
* kein genormter Zeichensatz/Zeichenkodierung. UTF-8 ist üblich heute unter Unix
* `std::string_view` (C++17) od. `boost::string_view` (C++11/14)

### Container
https://en.cppreference.com/w/cpp/container
* Heterogene Container:
  * `std::vector<Base*>` vs. `std::vector< shared_ptr<Base> >` vs `boost::ptr_vector<Base>`
  * Vererbung: `vector<Base>` ist _nicht_ verwandt mit `vector<Derived>`

### Algorithmen
https://en.cppreference.com/w/cpp/algorithm
* Ranges über Iteratoren
* Input-Range i.d.R. begrenzt (begin, end), Output i.d.R. unbegrenzt.

