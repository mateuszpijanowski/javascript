# Typy

Realnie każda wartość w javascript (oprócz `null` i `undefined`) jest obiektem (posiada własne metody, prototypy i relacje). Dzieje sie tak dzięki wykorzystaniu określonych konstruktorów w procesie deklaracji określonej wartości. 

W celu ułatwienia (a wrecz umożwliwienia) działania na danych i ich odróżniania javascript oferuje nam określone metody i operatory zwracające lub wskazujące bardziej szczegółowe typy danych do których należą:

- `Boolean` -> true oraz false.
- `null` -> specjalne słowo kluczowe oznaczające wartość zerową. Ponieważ w języku JavaScript rozróżniana jest wielkość liter, null nie jest tym samym co Null, NULL lub jakikolwiek inny wariant.
- `undefined` -> Wartość nieokreślona.
- `Number` -> np. 42 lub 3.14159.
- `String` - np. "elo"
- `Symbol` (nowość w ECMAScript 6) -> Typ danych, gdzie przykłady są niepowtarzalne i niezmienne.
- `Object` -> obikety i podtypy (funkcje i tablice)

Gdzie `Object` to typ złożony, pozostałe typy to tzn. typy prymitowne (nie rozkładjące się na podtypy).

## Określanie typów danych

- `typeof [wartość]` -> operator zwracający określony typ wskazanej wartości. Przykład:
```
const elo = 'test'
typeof elo // String
```

- `Array.isArray([wartość])` -> zwracająca informacje o tym czy wskzana wartość jest tablicą (rozróznianie typu złożonego). Przykład:
```
const elo = []
Array.isArray(elo) // true
```

- `[wartość] instanceof [Konstruktor]` -> testowanko czy określona wartość została 'wytowrzona' z użyciem wskazanego konstruktora (umożliwia określnaie typu funkcji. Przykład:
```
function elo() {}
console.log(elo instanceof Function) // true
```
\*Domyślnie wszystkie funkcje są tworzone z wykorzystaniem konstruktora Function (analogicznia to wygląda dla danych typów np. String(), Number() itd.)

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
- arguments -> lista argumantów funkcji w postaci tablico-podobnego obiektu
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
