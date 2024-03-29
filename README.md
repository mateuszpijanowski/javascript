**JavaScript** jak każdy inny język programowania to spory kawał wiedzy, definicji, sformułowań i logiki (lub jej braku). Wiedza ta lubi szybko wypadać z głowy i być niespodziewanie potrzebna (np. przed rozmową rekrutacyjną) lub gdy rzeczywiście, zamiast grać w gierki musimy w pracy zrobić jakiegoś taska i odpalić zakurzonego ŁepStorma czy innego VS kota.

W związku z tym poniższa ściąga ma za zadanie szybko przypomnieć nam wybrane obszary informacji dotyczących funkcjonalności i logiki wspaniałego języka, jakim jest JS! (lub przypomnieć nam, dlaczego jest on aż tak dziwny (a jest dziwny)).

**UWAGA!** W celu skrócenia ilości informacji, a także zwiększenia jej przejrzystości poniższa ściąga wykorzystuje wiele uproszczeń i skrótów (po szczegółowe rozwinięcia poszczególnych tematów zapraszam do źródeł). Sam tekst jest raczej pisany dla kogoś, kto już co nieco kazał zrobić komputerowi za pomocą kolorowych literek i podstawowa wiedza z zakresu programowania, jak i samego JavaScriptu jest tutaj wymogiem do pełnego zrozumienia poniższych opisów. 

**Wszelkie błędy językowe, logiczne i merytoryczne niestety są tutaj możliwe i proszę mieć to na uwadze!** Gdy zauważysz tego typu błąd, proszę, stwórz nowy `pull request` z poprawką, dziękuję :)

Podczas korzystania z poniższej ściągki zachęcam do korzystania z narzędzi pokroju https://codepen.io dla lepszego zrozumienia i przepracowania danych tematów.

**Miłej zabawy!**

# Spis treści

- [TYPY](#typy)
  - [Określanie typów](#określanie-typów-danych)
- [ZMIENNE](#zmienne)
  - [var](#var)
    - [Zakres funkcyjny](#scoping-zakres-funkcyjny)
    - [Hoisting](#hoisting-opis)
  - [let](#let)
    - [Zakres blokowy](#scoping-zakres-blokowy)
  - [const](#const)
- [FUNKCJE](#funkcje)
  - [syntax function](#deklaracja-po-przez-syntax-function)
  - [Funkcje strzałkowe](#deklaracja-funkcji-strzałkowej)
  - [this](#obiekt-this)
    - ['use strict'](#use-strict)
      - [Co zmienia 'use strict'?](#co-zmienia-use-strict)
        - [Zmiana pomyłek w kodzie na błędy](#zmiana-pomyłek-w-kodzie-na-błędy)
        - [Przypisywanie wartości do nie istniejących zmiennych](#przypisywanie-wartości-do-nie-istniejących-zmiennych)
        - [Nieudane próby przypisania wartości](#nieudane-próby-przypisania-wartości)
        - [Usuwanie właściwości obiektów](#usuwanie-właściwości-obiektów)
        - [Duplikowanie argumentów funkcji](#duplikowanie-argumentów-funkcji)
        - [Duplikowanie wartości obiektu](#duplikowanie-wartości-obiektu)
      - [Czy powinniśmy używać 'use strict'](#czy-powinniśmy-używać-use-strict)
    - [Funkcje strzałkowe](#funkcje-strzałkowe)
    - [call(), apply(), bind()](#call-apply-bind)
      - [call()](#call)
      - [apply()](#apply)
      - [bind()](#bind)
- [KLASY (todo)](#klasy-oop)
- [METODY (todo)](#metody)
- [PROGRAMOWANIE FUNKCYJNE (todo)](#programowanie-funkcyjne-fp)
- [ASYNCHRONICZNOŚĆ (todo)](#asynchroniczność)
- [MODUŁY I PAKIETY (todo)](#moduły-i-pakiety)
- [OBSŁUGA BŁĘDÓW (todo)](#obsługa-błędów)
- [ŹRÓDŁA](#źródła)

# Typy

Realnie każda wartość w JavaScript (oprócz `null` i `undefined`) jest obiektem (posiada własne metody, prototypy i relacje). Dzieje się tak dzięki wykorzystaniu określonych konstruktorów w procesie deklaracji wartości. 

W celu ułatwienia (a wręcz umożliwienia) działania na danych i ich odróżniania JavaScript oferuje nam określone metody i operatory zwracające lub wskazujące bardziej szczegółowe typy danych, do których należą:

- `Boolean` -> true oraz false.
- `null` -> specjalne słowo kluczowe oznaczające wartość zerową. Ponieważ w języku JavaScript rozróżniana jest wielkość liter, null nie jest tym samym co Null, NULL lub jakikolwiek inny wariant.
- `undefined` -> Wartość nieokreślona.
- `Number` -> np. 42 lub 3.14159.
- `String` - np. "elo".
- `Symbol` (nowość w ECMAScript 6) -> Typ danych, gdzie przykłady są niepowtarzalne i niezmienne.
- `Object` -> obikety i podtypy (funkcje i tablice).

Gdzie `Object` to typ złożony, pozostałe typy to tzn. typy prymitywne (nie rozkładające się na podtypy).

## Określanie typów danych

- `typeof [wartość]` -> operator zwracający określony typ wskazanej wartości. Przykład:
```
const elo = 'test'
typeof elo // String
```

- `Array.isArray([wartość])` -> metoda zwracająca informacje o tym, czy wskazana wartość jest tablicą (rozróżnianie typu złożonego). Przykład:
```
const elo = []
Array.isArray(elo) // true
```

- `[wartość] instanceof [Konstruktor]` -> testowanko czy określona wartość została 'wytworzona' z użyciem wskazanego konstruktora (umożliwia określanie typu funkcji). Przykład:
```
function elo() {}
console.log(elo instanceof Function) // true
```
\*Domyślnie wszystkie funkcje są tworzone z wykorzystaniem konstruktora ``Function`` (analogicznie to wygląda dla danych typów np. ``String()``, ``Number()`` itd.)

# Zmienne

## VAR
Podstawowa deklaracja zmiennej podlegająca modyfikacją.

### Cechy charakterystyczne:

#### Scoping (zakres funkcyjny)
`var` posiada zakres funkcyjny, przez co dostęp do niego mamy w każdym miejscu wewnątrz funkcji. Przykład:

```
function elo() {
  if (true) {
    var a = 'test'
  }
  
  console.log(a) // 'test'
}
```

#### Hoisting (opis)
`var` podlega hoistingow, co oznacza że kompilator js'a zanim zadeklaruje jej ostateczną wartość wskazaną w kodzie, utworzy zmienną z wartością `undefined`. Przykład:

```
console.log(elo) // undefined
var elo = 'test'
console.log(elo) // 'test'
```

#### Dodawanie wartości do globalnego obiektu
`var` dodaje wartość do globalnego obiektu (global object -> `window`). Przykład:

```
var elo = 'test'
console.log(window.elo) // 'test'
```

#### Redeklaracje
`var` umożliwia bezkrane redekleracje i nadpisywanie zmiennych. Przykład:

```
var elo = 'test'
var elo = 'test123' // brak błędu
console.log(elo) // 'test123'
```

## LET
Deklaracja zaminnej podlegającej modyfikacją dodana w ES6.

### Cechy charakterystyczne:

#### Scoping (zakres blokowy)
`let` posiada zakres blokowy, przez co dostęp do niego mamy tylko wewnątrz określonego bloku (`{}`). Przykład:

```
function elo() {
  let a = 'elo'

  if (true) {
    let b = 'test'
  }
  
  console.log(a) // 'elo'
  console.log(b) // ERROR
}
```

#### Hoisting
`let` **nie** podlega zasadzie hoistingu. Przykład:

```
console.log(elo) // ERROR
let elo = 'test'
console.log(elo) // 'test'
```

#### Dodawanie wartości do globalnego obiektu
Zmienne `let` nie zostają dodane do globalnego obiektu (global object -> `window`). Przykład:

```
let elo = 'test'
console.log(window.elo) // undefined
```

#### Redeklaracje
`let` **uniemożliwia** redekleracje i nadpisywanie zmiennych. Przykład:

```
let elo = 'test'
let elo = 'test123' // ERROR
```

## CONST
Deklaracja stałej uniemożliwiająca bezpośrednią modyfikację wartości. Cechy charakterystyczne zmiennej `let`.

# Funkcje

## Sposoby deklaracji funkcji

### Deklaracja po przez syntax `function`
Przykład:

```
function elo() { ... }
```

Tak zadeklarowana funkcja posiada charakterystykę deklaracji zmiennej ``var``.

#### Posiadane wartości i cechy
- this -> referencja do obiektu w którym wskazana funkcja została wykonana (domyślnie będzie to globalny obiekt window)
- call -> body funkcji
- arguments -> lista argumantów funkcji w postaci **tablico-podobnego** obiektu
- name -> nazwa funkcji
- lenght -> liczba oczekiwanych argumentów
- caller -> wskazanie fn nadrzędnej (tej wewnątrz której została wykonana nasza funkcja)
- umożliwia użycie syntaxu `super()`
- umożliwia utworzenie konstruktowa
- umożliwa użycie pseudo-wartości `new.target`

### Deklaracja funkcji strzałkowej
Przykład:

```
() => { ... }
```

#### Posiadane wartości i cechy
- brak własnego obiektu this -> zawsze jest to referencja do nadrzędnego `this`
- call -> body funkcji
- brak obiektu `arguments` i `caller`
- name -> nazwa funkcji
- lenght -> liczba oczekiwanych argumentów

\* Funkcja strzałkowa ma cechy zwykłej funkcji rozszerzonej o działanie syntax'u `'use strict'`.

## Obiekt this

Zacznijmy od definicji `this` - jest to referencja do obiektu w kontekście którego została wykonana funkcja, wewnątrz której używamy `this`. Początkowo może nie brzmieć to zbyt jasno, ale proste przykłady powinny rozwiać wszelkie wątpliwości.

`obj.someFn(this)` -> `this` w tym przypadku zwróci nam `obj`

Spójrzmy na bardziej rozbudowany przykład:

```
const obj = {
  name: 'Adam',
  age: 20,
  getData() {
    return `${this.name} ma ${this.age} lat.`
  },
}

obj.getData() // Adam ma 20 lat.
```

Ten prosty przykład powinien dokładnie zobrazować co dokładnie zwraca nam obiekt `this` i jakie daje nam możliwości.

Domyślnie `this` wskazuje nam obiekt `window` (zakładając, że mówimy o webowym wykorzystaniu JS'a) -> `this === widnow` // true

```
function a() {
  return this
}

a() // window object
```

Dzieje się tak, ponieważ wywołanie `a()` w globalnym kontekście to nic innego jak `window.a()`.

## 'use strict'

Spójrzmy na taki przykład:

```
const obj = {
  name: 'Adam',
  age: 20,
  getData() {
    var getName = function() {
      return this.name
    }
    return `${getName()} ma ${this.age} lat.`
  }
}

obj.getData() // undefined ma 20 lat.
```

Mamy tutaj pewien problem, mianowicie chcielibyśmy, aby nasza funkcja zwróciła nam to samo co funkcja z wcześniejszego przykładu, jednak zamiast zwrotki 'Adam' z funkcji `getName` otrzymujemy `undefined`. Dlaczego tak się dzieje? Spójrzmy dokładniej na kontekst wykonania funkcji `getName`.

```
return `${getName()} ma ${this.age} lat.`
```

Jak możemy zauważyć, przed nazwą funkcji nie mamy nic, więc kierując się wcześniejszymi informacjami, możemy założyć, że `getName()` to inaczej `window.getName()` i rzeczywiście to jest przyczyna naszego problemu. Miejsce wykonania funkcji nie ma w tym przypadku żadnego znaczenia, liczy się referencja do obiektu nadrzędnego. Tutaj nie mam takiej referencji więc domyślnie `this` zwraca nam obiekt globalny `window`, a obiekt `window` nie ma wartości `name` (lub ma ale inną niż my chcemy definiować).

Chyba możemy się zgodzić, że nie jest to rozwiązanie zbyt intuicyjne. W związku z tym kolejne wersje JS'a otrzymały możliwość rozwiązania tego problemu dzięki wykorzystaniu syntax'a `'use strict'` deklarowanego na początku funkcji lub skryptu. Spójrzmy, co się stanie, gdy użyjemy tej wartości.

```
const obj = {
  name: 'Adam',
  age: 20,
  getData() {
    'use strict'
    var getName = function() {
      return this.name // Cannot read properties of undefined (reading 'name') 
    }
    return `${getName()} ma ${this.age} lat.`
  }
}

obj.getData()
```

Wykonanie naszego kodu skończy się na błędzie "Cannot read properties of undefined (reading 'name')". Dzieje się tak, ponieważ `'use strict'` zapobiega przypisaniu obiektu `this` domyślnej wartości `window` i tym samym wymaga od nas wykorzystywania `this` tylko w odpowiednim kontekście, które zawiera referencje do obiektu nadrzędnego.

`'use strict'` w tym przypadku nie do końca rozwiązuje nasz problem, ale zanim przejdziemy do tego, co nam tutaj rzeczywiście pomoże, chciałbym pochylić się trochę, nad tym, co dokładnie zmienia `'use strict'` i dlaczego.

### Co zmienia 'use strict'?

Tryb "ścisły" powstał w aktualizacji ES5 i miał za zadanie załatać kilka problemów JS'a, a także włączyć tryb, w którym kompilator nie przepuści nam wszystkich głupot, które mogliśmy wymyślić w naszych kolorowych literkach. Tryb ten definiujemy dla całego skryptu, funkcji, modułu lub dla konkretnej klasy i tylko wskazany obszar kodu będzie objęty restrykcyjnymi zasadami.

Warto zaznaczyć, że nie każda wersja danej przeglądarki wspiera `'use strict'` w związku z tym poleganie na nim nie zawsze było pewne i zależało od środowiska, w którym kod był testowany/kompilowany.

No dobra wiemy, po co nam ten tryb i jak go włączyć, teraz pytanie, co dokładnie on zmienia.

#### Zmiana pomyłek w kodzie na błędy

Poznaliśmy już kilka dziwnych zachować JS'a, które kompilator ignorował i głaskał nas po główce. `'use strict'` wprowadza nam tutaj pewną rewolucję, mianowicie wiele z tych "pomyłek" przetwarza na realne błędy, przez co wymusza na nas zmianę logiki średnio napisanego kodu wykorzystującego luki JS'a lub wskazuje nam konkretne miejsce, gdzie mogliśmy (poprzez wady języka) popełnić błąd. Taki przykład był podany na początku rozdziału o `'use strict'`.

#### Przypisywanie wartości do nie istniejących zmiennych

```
let fajnaZmienna;
fajnaZmiena = 'elo';
```

Widać tutaj coś podejrzanego? Jeżeli nie to gratulacje kompilator ma takie samo zdanie! Przyjrzyjcie się dokładnie, jak została napisana zmienna w drugim wierszu. Jeżeli dalej nie widzicie, to podpowiem, że według słownika języka polskiego powinniśmy pisać zmie**NN**a przez dwa "n". 

No tak standardowy błąd podczas pisania kodu pomyłki językowe, zgubione literki czy jakieś inne głupoty wynikające z 3 dnia ciągłej pracy przed deadlinem to codzienność. Problem polega na tym, że w kontekście debuggowania dużego fragmentu kodu będzie nam bardzo ciężko dotrzeć do tego, gdzie popełniliśmy błąd i że polega on na brakującej liternce w zmiennej.

```
'use strict'

let fajnaZmienna
fajnaZmiena = 'elo' // Uncaught ReferenceError: fajnaZmiena is not defined
```

Użycie `'use strict'` naprawia nam tutaj problem (a w zasadzie go tworzy, ale nieco szybkiej i z konkretnym wskazaniem).

#### Nieudane próby przypisania wartości

W JS'ie istnieje kilka scenariuszy, w którym możemy chcieć przypisać do czegoś wartość, a realnie tego nie zrobimy i dodatkowo nikt nam o niczym nie powie. Spójrzcie na poniższe przykłady:

```
var NaN = 5
console.log(fajnaLiczba) // NaN

var undefined = 'wcale nie bo coś tutaj deklaruje!'
console.log(undefined) // undefined

const obj = {
  get wiek() {
    return 20
  },
}
obj.wiek = 15
console.log(obj.wiek) // 20
```

Jak widać, powyżej wszystkie próby przypisania wartości nic nie zmieniają. To trochę problem, ponieważ podobnych przykładów jest całkiem sporo, a JS nic nam nie mówi. Dla niego, jak i dla nas w momencie pisania tego kodu wszystko jest spoko.

Jak możemy się domyślić, rozwiązaniem tego problemu jest zadeklarowanie `'use strict'` w pierwszej linijce skryptu, dzięki czemu w każdej z tych prób JS rzuci nam błędem z dokładnym opisem, co poszło nie tak.

#### Usuwanie właściwości obiektów

Paczaj co tutaj się wyprawia:

```
var noFajnaZmienna = 'tak tak jestem fajną zmienna'
delete noFajnaZmienna
console.log(noFajnaZmienna) // 'tak tak jestem fajną zmienna'

delete Object.call
console.log(Object.call) // function ...

var fajnaTablica = []
delete fajnaTablica.length
console.log(fajnaTablica.length) // 0
```

Tutaj ponownie próbujemy zrobić jakieś nielegalne rzeczy, ale nic nam z tego nie wychodzi i nawet o tym nie wiemy (wszystko by się udało gdyby nie ten wścibski `console.log`!).
Dodanie `'use strict'` rozwiązuje nasz problem i ponownie w każdym z tych miejsc otrzymamy fajne error'y.

#### Duplikowanie argumentów funkcji

```
function jestemFunkcjaKtoraDodajeArgumenty(a, b, b) {
  return a + b + b // po co deklarować nowe literki jak można korzystać z tych samych?
}
```

Ten jakże ciekawy przykład pokazuje kolejny prosty do osiągnięcia błąd od strony logiki naszego kodu. Dodanie `'use strict'` rzuci nam tutaj błędem wskazującym na podwójną deklarację tego samego argumentu.

#### Duplikowanie wartości obiektu

```
var obj = {
  name: 'Adam',
  name: 'Stefan',
}

console.log(obj.name) // Stefan wygrał
```

Kolejny błąd i kolejne ignorowanie go ze strony JS'a. Oczywiście dodanie `'use strict'` zwróci nam tutaj odpowiedni błąd.

### Czy powinniśmy używać 'use strict'

Hah co za głupie pytanie przecież powyższe argumenty chyba jasno wskazują na odpowiedź! Zanim jednak do niej przejdziemy, warto dodać, że nie wypisałem tutaj wszystkich zmian, jakie wprowadza nam użycie `'use strict'`. Mimo to wydaje mi się, że wypisałem najważniejsze z nich wraz ze wcześniejszą modyfikacją działania obiektu `this`. Po więcej zapraszam do źródeł.

Wróćmy teraz do pytania i do odpowiedzi która brzmi:

- **NIE** (choć to zależy)

Dlaczego?! Kolejna odpowiedź będzie nieco dłuższa, ale podobnie prosta - większość zmian wprowadzanych przez `'use strict'` zostaje automatycznie zaimplementowana podczas używania standardów wprowadzonych w aktualizacji ES6 takich jak, chociażby deklarowanie zmienny przy użyciu `let` i `const` zamiast `var` czy używania funkcji strzałkowych. Dodatkowo używanie modułów domyślnie wprowadza w ich zakresie działania zasady `'use strict'`. Podobnie sprawa wygląda w przypadku klas.

No dobra, ale dopisałem do wielkiego **NIE**, że to zależy. W rzeczywistości rzeczywiście może to zależeć choćby od tego, z jakim kodem mamy do czynienia. W naszej pracy nie rzadko wpadamy w legacy kod napisany przez kogoś ze środkowej Azji 10 lat temu. Wtedy użycie `'use strict'` w konkretnych pomniejszych obszarach kodu może nam pomóc szybko wyłapać patologie w kolorowych literkach i szybko je załatać.

Tak to już jest z naszym kochanym JS'em nie ma tutaj jasnych i prostych odpowiedzi czy czegoś używać, czy też nie. Jak zwykle wszystko zależy i jak zwykle kluczem do dobrych decyzji jest wiedzy, czym różnią się dane podejścia.

## Funkcje strzałkowe

No dobra wracając do naszego `this` - `'use strict'` nie rozwiązuje do końca naszego problemu. Celem naszej funkcji jest zwrócenie tekstu "Adam ma 20 lat.", a nie rzucenie błędem mówiącym nam, że nie umiemy w kolorowe literki. Ostatecznym rozwiązaniem tego problemu są funkcje strzałkowe wprowadzone w wersji ES6. W opisie funkcji strzałkowej możemy przeczytać:

```
- brak własnego obiektu this -> zawsze jest to referencja do nadrzędnego `this`
```

Dzięki tej definicji możemy przerobić nasz przykład, tak by wreszcie zwracam nam to, czego potrzebujemy:

```
const obj = {
  name: 'Adam',
  age: 20,
  getData() {
    const getName = () => {
      return this.name
    }
    return `${getName()} ma ${this.age} lat.`
  }
}

console.log(obj.getData()) // Adam ma 20 lat.
```

I vuala nasz kod wreszcie robi to, czego od początku oczekiwaliśmy. Warto w tym miejscu pamiętać, o tym, że wykorzystanie funkcji strzałkowej a zwykłej deklaracji powinno być kontekstowe (to nie tak, że funkcja strzałkowa jest najlepsza i tylko jej używamy).
Kluczem jest tutaj wiedza, czym te deklaracje się różnią i którą z nich użyć w danym przypadku.

```
const obj = {
  name: 'Adam',
  age: 20,
  getData: () => { // EJ! Jeżeli pozbawimy tę funkcję własnego kontekstu this to przecież this wewnątrz tej funkcjie nie wskaże nam obiektu obj tylko nadrzędny obiekt this którym w tym przypadku będzie window!
    const getName = () => {
      return this.name
    }
    return `${getName()} ma ${this.age} lat.`
  }
}

console.log(obj.getData()) // undefined ma undefined lat.
```

No dobra a co jeżeli chcemy "narzucić" danej funkcji określony obiekt `this`?

## call(), apply(), bind()

Wbudowane w funkcje metody `call()`, `apply()`, `bind()` umożliwiają nam manipulowanie wartością obiektu `this` wewnątrz wskazanych funkcji.

### call()

Zacznijmy od przykładu:

```
const wizard = {
  name: 'Merlin',
  health: 50,
  heal() {
    return this.health = 100
  }
}

wizard.heal() // 100
console.log(wizard) // [...] health: 100, [...]
```

Prosty przykład, który prezentuje nam wykorzystanie this do zmiany wartości określonego pola w obiekcie. Co, jednak jeżeli dorzucimy kolejną postać (obiekt) do naszej gry, która sama nie posiada funkcji `heal()`, ale chciałaby skorzystać z tej umiejętności naszego maga? Tutaj z pomocą przychodzi nam metoda `call()`.

```
const wizard = {
  name: 'Merlin',
  health: 50,
  heal() {
    return this.health = 100
  }
}

const archer = {
  name: 'Robin Hood',
  health: 30,
}

wizard.heal.call(archer) // 100
console.log(archer) // [...] health: 100, [...]
```

Ciekawe prawda? **Call** umożliwia nam wykonanie wskazanej funkcji ze zmienionym obiektem `this` na to, co przekażemy w jej argumencie. Dzięki temu jesteśmy w stanie wykorzystywać zdolność (funkcję) jednej postaci (obiektu) na drugiej postaci (drugim obiekcie z innymi danymi bez pierwotnej funkcji).

No dobra a co jeżeli nasza funkcja `heal()` wyglądałaby tak:

```
heal(hp) {
  return this.health += hp
}
```

W jaki sposób teraz możemy ją wykorzystać w innym obiekcie przy jednoczesnym przekazaniu wartości argumentu `hp`?

```
wizard.heal.call(archer, 50) // 80
console.log(archer) // [...] health: 80, [...]
```

I oczywiście działa to analogicznie z kolejnymi argumentami (wystarczy wymieniać je po przecinku przy zachowaniu zasady -> pierwszy argument metody **call** to `this`, a każdy kolejny to następny argument wskazanej funkcji).

### apply()

Metoda ``apply()`` cechuje się tym samym, co metoda ``call()`` z jedną drobną różnicą. W przypadku metody **call** wymienialiśmy argumenty funkcji po przecinku po pierwszym argumencie będącym obiektem `this` (.call(archer, 50, ...) natomiast w przypadku metody **apply** używamy tablicy do deklarowania listy argumentów wskazanej funkcji (.apply(archer, [50, ...]).

### bind()

Metoda `bind()` działa tak jak metoda `call()`, ponownie z jedną różnicą. W przypadku metody **call** i **apply** funkcja, na której używaliśmy tej metody, zostaje automatycznie wykonana (bez względu na kontekst np. przypisywanie do zmiennej) natomiast metoda **bind** tworzy nową deklarację funkcji ze zmienionym obiektem `this`, którą to deklaracje możemy przechowywać w zmiennej i wykorzystać w przyszłości:

```
const wizard = {
  name: 'Merlin',
  health: 50,
  heal(hp) {
    return this.health += hp
  }
}

const archer = {
  name: 'Robin Hood',
  health: 30,
}

const healArcher = wizard.heal.bind(archer, 50)
console.log(archer) // [...] health: 30, [...]

healArcher() // 80
console.log(archer) // [...] health: 80, [...]
```
# Klasy (OOP)

# Metody

# Programowanie funkcyjne (FP)

# Asynchroniczność

# Moduły i pakiety

# Obsługa błędów

# Źródła

- https://developer.mozilla.org/
- https://www.w3schools.com/
- JavaScript: The Advanced Concepts (2023 Update) - https://www.udemy.com/course/advanced-javascript-concepts/
