# Przechowywanie kwot pieniężnych w groszach

## Wprowadzenie

W świecie aplikacji finansowych precyzja ma kluczowe znaczenie. Nawet najmniejszy błąd w zaokrągleniu może prowadzić do poważnych konsekwencji, zwłaszcza gdy mówimy o operacjach na dużą skalę. Historia zna przypadki, w których błędy obliczeniowe doprowadziły do ogromnych strat finansowych – na przykład w 2003 roku firma Woolworths w Australii straciła miliony dolarów z powodu błędu w systemie przetwarzania płatności, który wynikał z problemów z zaokrąglaniem liczb zmiennoprzecinkowych.

Takie problemy mogą wystąpić również w bankowości – np. w latach 80. pewien amerykański bank przez lata nieświadomie tracił setki tysięcy dolarów, ponieważ różnice wynikające z błędnego zaokrąglania kumulowały się na kontach klientów. Dlatego tak ważne jest, aby przechowywać wartości pieniężne w sposób, który minimalizuje ryzyko błędów.

## Dlaczego przeliczanie do groszy ma sens?

- Brak błędów zaokrągleń: Operacje na liczbach zmiennoprzecinkowych (np. float) mogą prowadzić do subtelnych błędów zaokrągleń. Przechowywanie kwot w groszach (czyli liczbach całkowitych i najmniejszej liczbie walutowej danego kraju) pozwala uniknąć tych problemów.
- Wydajność baz danych: Większość baz danych efektywniej przetwarza liczby całkowite niż zmiennoprzecinkowe. To może mieć znaczenie, zwłaszcza przy dużej liczbie transakcji.
- Zgodność z API płatności: Wiele systemów płatności, jak Stripe czy PayPal, również operuje na najmniejszych jednostkach walutowych. Integracja z nimi jest prostsza, jeśli stosujemy podobne podejście.

## Przykład implementacji

Wyobraźmy sobie prostą aplikację, gdzie użytkownicy mogą dodawać transakcje w złotówkach. Po stronie frontendu, używając czystego JavaScriptu, kwoty będą wprowadzane w złotych (np. 12.34), ale przeliczymy je na grosze (1234) przed zapisaniem w bazie danych MySQL.

### Konwersja kwoty do groszy

Funkcja `toCents` przyjmuje wartość w złotówkach (np. 12.34) i zwraca odpowiadającą liczbę groszy (1234).

```javascript
function toCents(amount) {
  if (typeof amount !== 'number' || isNaN(amount)) {
    throw new Error('Invalid amount');
  }
  return Math.round(amount * 100);
}

// Przykłady użycia
console.log(toCents(12.34)); // 1234
console.log(toCents(0.99));  // 99
console.log(toCents(100));   // 10000
```

### Formatowanie groszy na złotówki

Funkcja `formatCurrency` konwertuje liczbę groszy na czytelną wartość w złotówkach i formatuje ją jako string.

```javascript
function formatCurrency(cents) {
  if (!Number.isInteger(cents)) {
    throw new Error('Invalid cents value');
  }
  return (cents / 100).toFixed(2) + ' PLN';
}

// Przykłady użycia
console.log(formatCurrency(1234)); // "12.34 PLN"
console.log(formatCurrency(99));   // "0.99 PLN"
console.log(formatCurrency(10000)); // "100.00 PLN"
```

### Operacje matematyczne na groszach

Przechowywanie kwot w groszach ułatwia wykonywanie operacji matematycznych, takich jak mnożenie i dzielenie przez liczby całkowite.

#### Mnożenie

Funkcja `multiplyCents` mnoży wartość w groszach przez liczbę całkowitą (np. dla naliczenia rabatu lub podatku).

```javascript
function multiplyCents(cents, multiplier) {
  if (!Number.isInteger(cents) || !Number.isInteger(multiplier)) {
    throw new Error('Invalid input values');
  }
  return cents * multiplier;
}

// Przykłady użycia
console.log(multiplyCents(1234, 2)); // 2468
console.log(multiplyCents(990, 3));  // 2970
console.log(multiplyCents(10000, 5)); // 50000
```

#### Dzielenie

Funkcja `divideCents` dzieli wartość w groszach przez liczbę całkowitą, zwracając wynik zaokrąglony w dół do najbliższego grosza.

```javascript
function divideCents(cents, divisor) {
  if (!Number.isInteger(cents) || !Number.isInteger(divisor) || divisor === 0) {
    throw new Error('Invalid input values');
  }
  return Math.floor(cents / divisor);
}

// Przykłady użycia
console.log(divideCents(1234, 2)); // 617
console.log(divideCents(990, 3));  // 330
console.log(divideCents(10000, 4)); // 2500
```

## Baza danych (MySQL)

W MySQL tworzymy tabelę `transactions` z kolumną `amount` typu `INT`. Dzięki temu mamy pewność, że wszystkie kwoty będą zapisywane w groszach. INT przechowa nie więcej niż 2147483647 groszy, czyli około 214 milionów PLN.

```sql
CREATE TABLE transactions (
  id INT AUTO_INCREMENT PRIMARY KEY,
  amount INT NOT NULL
);
```

## Podsumowanie

Przechowywanie walut w najmniejszej jednostce to prosta, ale skuteczna technika, która pozwala uniknąć wielu pułapek związanych z operacjami finansowymi. Niezależnie od tego, czy budujesz prostą aplikację do śledzenia wydatków, czy zaawansowaną platformę e-commerce, warto rozważyć to podejście.