# Głębokie spojrzenie na system reaktywności Vue.js

Vue.js wykorzystuje **Virtual DOM (VDOM)** i **system reaktywności** do efektywnego aktualizowania interfejsu użytkownika na podstawie zmian danych. Oto jak działają te mechanizmy:

## Virtual DOM (VDOM)

Virtual DOM to lekka reprezentacja JavaScript rzeczywistego DOM dostarczana przez przeglądarkę. Gdy modyfikujesz dane w Vue.js, framework **nie manipuluje bezpośrednio DOM-em**. Zamiast tego aktualizuje najpierw Virtual DOM.

## Kompilacja szablonów

Vue.js kompiluje szablon każdego komponentu do funkcji renderujących. Podczas kompilacji szablon jest parsowany i przekształcany w funkcję render, która generuje elementy Virtual DOM. Proces ten optymalizuje wydajność renderowania, unikając parsowania szablonu przy każdej aktualizacji.

## System reaktywności

Vue.js śledzi zmiany danych i aktualizuje Virtual DOM dzięki systemowi reaktywności. Gdy definiujesz właściwość `data` w komponencie, Vue.js opakowuje ją w **getter i setter**. Te funkcje pozwalają frameworkowi obserwować zmiany właściwości danych.

## Śledzenie zależności

Podczas wykonywania funkcji render komponentu, Vue.js automatycznie rejestruje, które właściwości danych są używane. Tworzy to relację zależności między komponentem a danymi, na których się opiera.

## Reaktywne aktualizacje

Gdy właściwość danych, od której zależy komponent, ulegnie zmianie, Vue.js wyzwala ponowne renderowanie tego komponentu. Porównuje nowy Virtual DOM z poprzednim i oblicza minimalny zestaw manipulacji DOM potrzebnych do aktualizacji interfejsu.

## Algorytm diffingu

Vue.js używa **algorytmu diffingu**, aby efektywnie aktualizować rzeczywisty DOM na podstawie zmian w Virtual DOM. Porównuje nowy VDOM z poprzednim i aktualizuje tylko niezbędne części DOM, minimalizując liczbę operacji.

## Operacje patch

Algorytm diffingu wykonuje operacje patch na rzeczywistym DOM. Dodaje, usuwa lub aktualizuje węzły DOM zgodnie z potrzebami, aby dopasować strukturę do nowego Virtual DOM.

Dzięki Virtual DOM i systemowi reaktywności Vue.js zapewnia **płynne i reaktywne doświadczenie użytkownika**, minimalizując bezpośrednie interakcje z DOM i optymalizując wydajność.