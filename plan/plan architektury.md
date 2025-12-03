Cześć. Jako Twój partner technologiczny **SIMONS WEB**, przeanalizowałem Twoje decyzje. To bardzo dojrzałe podejście. Wybór "Component Swap" dla przełącznika to strzał w dziesiątkę – zademonstrujesz w ten sposób inżynierską precyzję i znajomość Vue, a nie tylko umiejętność zmiany stylów CSS.

Poniżej przedstawiam **Plan Architektury Technicznej** z uwzględnieniem Twoich wytycznych.

-----

### 1\. Architektura Funkcji "Killer Feature": Interactive Switch (Component Swap)

Zdecydowaliśmy się na pełną wymianę komponentów. To rozwiązanie jest trudniejsze, ale robi piorunujące wrażenie "aplikacji", a nie zwykłej strony.

**Jak to zbudujemy w Nuxt 4 / Vue 3:**

  * **Mechanizm:** Użyjemy dynamicznego komponentu `<component :is="activeComponent" />`.
  * **Wydajność (Kluczowa dla Twojej obietnicy szybkości):**
      * Aby nie obciążać strony przy pierwszym wejściu ("Lekarz"), komponenty alternatywne ("Trener", "Architekt") będą ładowane asynchronicznie (`defineAsyncComponent`), ale z opcją *prefetching* w tle.
      * Użyjemy `<KeepAlive>`, aby po przełączeniu z powrotem na "Lekarza", stan formularza lub scrolla był zachowany (płynność UX).
  * **Animacja:** Zastosujemy `<Transition>` z `mode="out-in"`, aby stary komponent płynnie znikał, zanim pojawi się nowy. To wyeliminuje "skakanie" treści (Layout Shift).

**Struktura kodu (Draft):**

```typescript
<script setup lang="ts">
import { shallowRef } from 'vue'; // shallowRef jest wydajniejszy dla komponentów
import HeroDoctor from '~/components/sections/HeroDoctor.vue';
import HeroTrainer from '~/components/sections/HeroTrainer.vue';

const currentView = shallowRef(HeroDoctor);
</script>

<template>
  <Transition name="fade" mode="out-in">
    <component :is="currentView" />
  </Transition>
</template>
```

-----

### 2\. Strategia Rozwoju: Master App vs. Nuxt Layers

Prosiłeś o rozpisanie obu podejść. To kluczowa decyzja biznesowa. Obecnie startujemy z **Podejściem A** (szybsze na start), ale architektura będzie gotowa na migrację do **Podejścia B** (skalowalność).

#### Podejście A: Master Application (Boilerplate / Clone)

*To jest Twoja obecna decyzja dla MVP i własnego portfolio.*

  * **Zasada działania:** Tworzysz jedno idealne repozytorium "Matkę" (Master). Kiedy przychodzi klient (Lekarz), klonujesz to repozytorium, zmieniasz treści w `content/`, podmieniasz kolory w `tailwind.config` i wrzucasz na Vercel.
  * **Zalety:**
      * **Totalna niezależność:** Jeśli Lekarz zażyczy sobie dziwnej funkcji, możesz ją wdrożyć bez psucia kodu u Trenera.
      * **Szybkość startu:** Nie musisz konfigurować NPM packages ani monorepo. Po prostu `git clone`.
  * **Wady:**
      * **Piekło utrzymania (Maintenance Hell):** Jeśli wykryjesz błąd w nagłówku (Header), musisz go ręcznie poprawić w 20 oddzielnych repozytoriach klientów.
      * **Dryf technologiczny:** Z czasem stare projekty zostają na starych wersjach Nuxta, a nowe są na nowszych.

#### Podejście B: Nuxt Layers (Dziedziczenie)

*To jest cel docelowy, gdy będziesz miał \>5 klientów.*

  * **Zasada działania:** Tworzysz bazowy pakiet `github:simons-web/core-theme`. Strona klienta jest prawie pusta – zawiera tylko plik konfiguracyjny, który mówi: "Działam na silniku Simons Web Core".
  * **Zalety:**
      * **Aktualizacje:** Poprawiasz błąd w `core-theme`, robisz push, i **wszystkie** strony klientów (po przebudowaniu na Vercel) otrzymują poprawkę automatycznie.
      * **Czystość:** Repozytorium klienta zawiera tylko jego unikalne treści (Markown) i konfigurację (Logo, Kolory). Zero kodu JS.
  * **Wady:**
      * Większy próg wejścia w konfigurację.
      * Trudniej wprowadzać bardzo specyficzne zmiany dla jednego klienta ("overrides").

[Image of Nuxt layers architecture diagram]

-----

### 3\. Nuxt Content v3 (Bez Studio na start)

Rezygnacja z Nuxt Studio w MVP to dobra decyzja – oszczędza czas konfiguracji. Skupimy się na:

1.  **Struktura plików:** Zaprojektujemy folder `content/` tak, aby był gotowy na przyszłe wdrożenie Studio.
      * `/content/1.pakiety/` (Twoja oferta)
      * `/content/2.blog/` (Twoje artykuły eksperckie)
      * `/content/config.yml` (Globalne ustawienia strony, jak telefon czy linki social media – dzięki temu edytujesz je w jednym miejscu).
2.  **Type Safety:** Wykorzystamy TypeScript do otypowania treści z Markdown. Dzięki temu w kodzie Vue będziesz miał podpowiadanie składni (np. `article.title`, `article.tags`).

-----

### 4\. Plan Działania (Roadmap)

Skoro mamy jasność co do architektury, proponuję podzielić pracę na 3 sprinty:

**Faza 1: Fundamenty (Setup & Core)**

  * Inicjalizacja projektu Nuxt 4 + Tailwind CSS.
  * Konfiguracja Vercel (CI/CD).
  * Implementacja nawigacji i stopki.
  * **Kamień milowy:** Pusta strona działająca na domenie produkcyjnej.

**Faza 2: "Killer Features" & Content**

  * Budowa **Interactive Switch** (Logika Component Swap).
  * Stworzenie komponentów "Lekarz" i "Trener" do demo.
  * Konfiguracja Nuxt Content v3 i wdrożenie treści z pliku `plan-architektury-tresci.md`.
  * **Kamień milowy:** Działająca strona z pełną treścią i demo.

**Faza 3: Optymalizacja i "Sales"**

  * SEO (Meta tagi, Sitemap, robots.txt).
  * Podpięcie formularzy (np. Formspree/Resend – zgodnie z planem).
  * Testy wydajności (Lighthouse).

-----

### Co robimy dalej?

Czy chcesz, abym teraz przygotował **szczegółową listę zadań (To-Do List)** dla Fazy 1, wraz z komendami do terminala, abyś mógł od razu uruchomić projekt?