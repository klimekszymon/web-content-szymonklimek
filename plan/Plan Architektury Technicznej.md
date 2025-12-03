# Plan Architektury Technicznej
**Wersja:** 1.0
**Data:** 03.12.2025
**Status:** Draft / Zatwierdzony do Fazy 1

---

## 1. Stos Technologiczny (Tech Stack)

Decyzje oparte na założeniach: Szybkość, Bezpieczeństwo, Brak Długu Technicznego.

| Warstwa | Technologia | Uzasadnienie |
| --- | --- | --- |
| **Framework** | **Nuxt 4** | Przyszłościowe rozwiązanie, najnowsze API Vue 3, lepsza wydajność hydratacji. |
| **Język** | **TypeScript** | Pełne typowanie dla bezpieczeństwa kodu i łatwiejszego utrzymania. |
| **CMS** | **Nuxt Content v3** | Git-based CMS. Treść jako kod. Brak bazy danych SQL. Gotowość pod Nuxt Studio. |
| **Styling** | **Tailwind CSS** | Utility-first. Szybkie prototypowanie. Łatwa obsługa Dark Mode. |
| **Hosting** | **Vercel** | Edge Network, Serverless Functions (dla API), Zero-config deployment. |
| **Poczta** | **Resend / Formspree** | Obsługa formularzy bez backendu. |

---

## 2. Kluczowe Wzorce Architektoniczne

### A. "Killer Feature": Dynamiczny Przełącznik Branż (Component Swap)
Mechanizm pozwalający na zmianę kontekstu strony (Lekarz -> Trener -> Architekt) bez przeładowania, z zachowaniem wysokiej wydajności.

* **Implementacja:** Wykorzystanie `<component :is="...">` z Vue 3.
* **Optymalizacja:** `shallowRef` dla komponentów (unikanie zbędnej reaktywności) + `defineAsyncComponent` (ładowanie kodu tylko gdy potrzebny).
* **UX:** `<KeepAlive>` do zachowania stanu (np. wpisanego tekstu) oraz `<Transition mode="out-in">` dla płynnych animacji.

### B. Strategia Skalowalności: Master App -> Nuxt Layers
Aktualnie (MVP) stosujemy podejście **Master App** (jedno repozytorium matka), ale struktura katalogów jest przygotowana pod migrację do **Nuxt Layers**.

* **Faza MVP:** Klonowanie repozytorium dla każdego klienta. Szybki start, pełna elastyczność.
* **Faza Growth (>5 klientów):** Wydzielenie `base-theme` do osobnej warstwy (NPM/Git), z której dziedziczą strony klientów. Umożliwi to masowe aktualizacje bezpieczeństwa.

---

## 3. Struktura Danych (Content Architecture)

Przygotowanie pod Nuxt Content v3. Separacja treści od logiki wizualnej.

```text
content/
├── 1.home/             # Sekcje strony głównej (Hero, Oferta)
│   ├── hero.yml        # Konfiguracja tekstów dla Hero
│   └── about.md        # Treść "O mnie"
├── 2.blog/             # Artykuły eksperckie
│   ├── post-1.md
│   └── post-2.md
├── 3.legal/            # RODO, Polityka Prywatności
└── settings.yml        # Globalne zmienne: Telefon, Email, Social Links
```

Zasada: W kodzie komponentów (np. Footer.vue) nie wpisujemy numeru telefonu na sztywno. Pobieramy go z queryContent('settings').

## 4. Mapa Rozwoju (Roadmap)
Faza 1: Fundamenty (Setup & Core)
[ ] Inicjalizacja repozytorium Nuxt 4 + Tailwind.

[ ] Konfiguracja ESLint + Prettier (Code Quality).

[ ] Podstawowy Layout (Header, Footer, Mobile Menu).

[ ] Deployment "Hello World" na Vercel (sprawdzenie CI/CD).

### Faza 2: "Killer Features" & Content
[ ] Implementacja mechanizmu SwitchComponent.vue.

[ ] Stworzenie makiety "Lekarz" i "Trener" (Visuals).

[ ] Konfiguracja Nuxt Content v3.

[ ] Migracja treści z plan-architektury-tresci.md do plików .md/yml.

### Faza 3: Optymalizacja i "Sales"
[ ] SEO: Generowanie Sitemap.xml, meta tagi (OG Tags).

[ ] Integracja formularza kontaktowego.

[ ] Audyt Lighthouse (cel: 100/100 Performance).

[ ] Uruchomienie domeny produkcyjnej.


---

### Co robimy teraz?

Plik jest gotowy. Sugeruję przejść do **Fazy 1: Fundamenty**.
Czy mam wygenerować listę komend terminala, abyś mógł zainicjować projekt Nuxt 4