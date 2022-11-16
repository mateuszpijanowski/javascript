**JavaScript** jak każdy inny język progrmowania to spory kawał wiedzy, definicji, sformuowań i logiki (lub jej braku). Wiedza ta lubi szybko wypadać z głowy i być niespodziewanie potrzebna (np. przed rozmową rekrutacyjną) lub gdy rzeczywiście zamiast grać w gierki musimy w pracy zrobić jakiegoś taska i odpalić zakurzonego ŁepStorma czy innego VS kota. 

W związku z tym poniższa ściąga ma za zadanie szybko przypomnieć nam wybrane obszary informacji dotyczących funkcjonalości i logiki wspaniałego języka jakim jest JS! (lub przypomnieć nam dlaczego jest on aż tak dziwny (a jest dziwny)). **Miłej zabawy!**

# Spis treści

- [TYPY](#typy)
  - [Określanie typów](#określanie-typów-danych)
- [ZMIENNE](#zmienne)
  - [var](#var)
  - [let](#let)
  - [const](#const)
- [FUNKCJE](#funkcje)
  - [syntax function](#deklaracja-po-przez-syntax-function)
  - [funkcje strzałkowe](#deklaracja-funkcji-strzałkowej)
  - [this](#obiekt-this)
    - [use strict](#use-strict)
    - [Funkcje strzałkowe](#funkcje-strzałkowe)
    - [call, apply, bind](#call-apply-bind)
      - [call](#call)
      - [apply](#apply)
      - [bind](#bind)
- [KLASY](#klasy-oop)
- [METODY](#metody)
- [PROGRAMOWANIE FUNKCYJNE](#programowanie-funkcyjne-fp)
- [ASYNCHRONICZNOŚĆ](#asynchroniczność)
- [OBSŁUGA BŁĘDÓW](#obsługa-błędów)

# Typy

Realnie każda wartość w JavaScript (oprócz `null` i `undefined`) jest obiektem (posiada własne metody, prototypy i relacje). Dzieje się tak dzięki wykorzystaniu określonych konstruktorów w procesie deklaracji wartości. 

W celu ułatwienia (a wrecz umożliwienia) działania na danych i ich odróżniania JavaScript oferuje nam określone metody i operatory zwracające lub wskazujące bardziej szczegółowe typy danych, do których należą:

- `Boolean` -> true oraz false.
- `null` -> specjalne słowo kluczowe oznaczające wartość zerową. Ponieważ w języku JavaScript rozróżniana jest wielkość liter, null nie jest tym samym co Null, NULL lub jakikolwiek inny wariant.
- `undefined` -> Wartość nieokreślona.
- `Number` -> np. 42 lub 3.14159.
- `String` - np. "elo".
- `Symbol` (nowość w ECMAScript 6) -> Typ danych, gdzie przykłady są niepowtarzalne i niezmienne.
- `Object` -> obikety i podtypy (funkcje i tablice).

Gdzie `Object` to typ złożony, pozostałe typy to tzn. typy prymitywne (nie rozkładjące się na podtypy).

## Określanie typów danych

- `typeof [wartość]` -> operator zwracający określony typ wskazanej wartości. Przykład:
```
const elo = 'test'
typeof elo // String
```

- `Array.isArray([wartość])` -> metoda zwracająca informacje o tym czy wskzana wartość jest tablicą (rozróznianie typu złożonego). Przykład:
```
const elo = []
Array.isArray(elo) // true
```

- `[wartość] instanceof [Konstruktor]` -> testowanko czy określona wartość została 'wytowrzona' z użyciem wskazanego konstruktora (umożliwia określnaie typu funkcji). Przykład:
```
function elo() {}
console.log(elo instanceof Function) // true
```
\*Domyślnie wszystkie funkcje są tworzone z wykorzystaniem konstruktora ``Function`` (analogicznia to wygląda dla danych typów np. ``String()``, ``Number()`` itd.)

# Zmienne

## VAR
Podstawowa deklaracja zmiennej podlegająca modyfikacją.

### Cechy charakterystyczne:

#### Scoping
`var` posiada zakres funkcyjny przez co dostep do niego mamy w każdym miejscu wewnątrz funkcji. Przykład:

```
function elo() {
  if (true) {
    var a = 'test'
  }
  
  console.log(a) // 'test'
}
```

#### Hoisting
`var` podlega hoistingow co oznacza że kompilator js'a zanim zadeklaruje jej ostateczą wartość wskazaną w kodzie, utworzy zmienną z wartością `undefined`. Przykład:

```
console.log(elo) // undefined
var elo = 'test'
console.log(elo) // 'test'
```

#### Dodawanie wartości do globalnego obiektu
`var` dodaje wartość do globalnego obiektu (global object -> window). Przykład:

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

#### Scoping
`let` posiada zakres blokowy przez co dostep do niego mamy tylko wewnątrz określonego bloku (`{}`). Przykład:

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
Zmienne `let` nie zostają dodane do globalnego obiektu (global object -> window). Przykład:

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
Deklaracja stałej unemożliwająca bezpośrednią modyfikacje wartości. Cechy charakterystyczne zmiennej `let`.

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

\* Funkcja strzałkowa posiada cechy zwykłej funkcji rozszerzonej o działanie syntax'u `'use strict'`.

## Obiekt this

Zacznijmy od definicji `this` - jest to referencja do okiektu w kotekście którego została wykonana funkcja wewnątrz której używamy `this`. Początkowo może nie brzmieć to zbyt jasno ale proste przykłady powinny rozwiać wszelkie wątpliwości.

obj.someFn(this) -> this w tym przypadku zwróci nam `obj`

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

Ten prosty przykład powienien dokładnie zobrazować co dokładnie zwraca nam obkiet `this` i jakie daje nam możliwości.

Domyślnie `this` w wskazuje nam obiekt window (zakładając że mówimy o webowym wykorzystaniu JS'a) -> this === widnow // true

```
function a() {
  return this
}

a() // window object
```

Dzieje się tak ponieważ wywłanie ``a()`` w globalnym kontekście to nic innego jak ``window.a()``.

## use strict

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

Mamy tutaj pewien problem, mianowicie chcielibyśmy aby nasza funkcja zwróciła nam to samo co funkcja z wcześniejszego przykładu, jednak zamiast zwrotki 'Adam' z funkcji `getName` otrzymujemy `undefined`. Dlaczego tak się dzieje? Spójrzmy dokładniej na kontekst wykonania funkcji `getName`.

```
return `${getName()} ma ${this.age} lat.`
```

Jak możemy zauważyć przed nazwą funkcji nie mamy nic, więc kierując się wcześniejszymi informacjami może założyć że `getName()` to inaczej `window.getName()` i rzeczywiście to jest przyczyna naszego problemu. Miejsce wykonania funkcji nie ma w tym przypadku żadnego znaczenia liczy się referencja do obiektu nadrzędnego. Tutaj nie mam takiej referencji więc domyśnie `this` zwraca nam obiekt `window`, a obiekt `window` nie posiada wartości `name`.

Chyba możemy się zgodzić że nie jest to rozwiązanie zbyt intuicyjne. W związku z tym kolejne wersje JS'a otrzymały możliwość rozwiązania tego problemu dzięki wykorzystaniu syntax'a 'use strict' deklarowanego na początku funkcji lub skryptu. Spójrzmy co się stanie gdy użyjemy tej wartości.

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

Wykonanie naszego kodu skończy się na błędzie "Cannot read properties of undefined (reading 'name')". Dzieje się tak ponieważ 'use strict' zapobiega przypiswaniu obiektu this domyślnej wartości `window` i tym samym wymaga od nas wykorzystywania `this` tylko w odpowiednim kontekście, które zawiera referencje do obiektu nadrzędnego.

## Funkcje strzałkowe

'use strict' nie rozwiązuje jednak naszego problemu. Celem naszej funkcji jest zwrócenie tesktu "Adam ma 20 lat.", a nie rzucenie błędem mówiącym nam że nie umiemy w kolorowe literki. Ostatecznym rozwiązaniem tego problemu są funkcje strzałkowe wprowadzone w wersji ES6. W opisie funkcji strzałkowej możemy przeczytać:

```
- brak własnego obiektu this -> zawsze jest to referencja do nadrzędnego `this`
```

Dzięki tej definicji możemy przerobić nasz przykład, tak by wreszcie zwracam nam to czego potrzebujemy:

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

I vuala nasz kod wreszcie robi to czego od początku oczekiwaliśmy. Warto w tym miejscu pamiętać, o tym że wykorzystanie funkcji strzałkowej a zwykłej deklaracji powinno być kontekstowe (to nie tak że funkcja strzałkowa jest najlepsza i tylko jej używamy). 
Kluczem jest tutaj wiedza czym te deklaracje się różnią i którą z nich użyć w danym przypadku.

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

Prosty przykła, który prezentuje nam wykorzystanie this do zmiany wartości określonego pola w obiekcie. Co jednak jeżeli dorzucimy kolejną postać (obiekt) do naszej gry która sama nie posiada funkcji heal(), ale chciała by skorzystać z tej umiejętności naszego maga? Tutaj z pomocą przychodzi nam metoda `call()`.

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

No dobra a co jeżeli nasza funkcja `heal()` wyglądała by tak:

```
heal(hp) {
  return this.health += hp
}
```

W jaki sposób terz możemy ją wykorzystać w innym obiekcie przy jednoczesnym przekazaniu wartości argumentu `hp`?

```
wizard.heal.call(archer, 50) // 80
console.log(archer) // [...] health: 80, [...]
```

I oczywiście działa to analogicznie z kolejnymi argumentami (wystarczy wymieniać je po przecinku przy zachowaniu zasady -> pierwszy argument metody **call** to `this`, a każdy kolejny to następny argument wskazanej funkcji).

### apply()

Metoda ``apply()`` cechuje się tym samym, co metoda ``call()`` z jedną drobą różnicą. W przypadku metody **call** wymienialiśmy argument funkcji po przecinku po pierwszym argumencie będącym obiektem `this` (.call(archer, 50, ...) natomiast w przypadku metody **apply** używamy tablicy do deklarowania listy argumentów wskazanej funkcji (.apply(archer, [50, ...]).

### bind()

Metoda `bind()` działa tak jak metoda `call()`, ponownie z jedną różnicą. W przypadku metody **call** i **apply** funkcja, na której używaliśmy tej metody zostaje automatycznie wykonana (bez względu na konktest np. przypisywanie do zmiennej) natomaiast metoda **bind** tworzy nową deklarację funkcji ze zmienionym obiektem `this`, którą to deklaracje możemy przechowywać w zmiennej i wykorzystać w przyszłości:

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

# Obsługa błędów
