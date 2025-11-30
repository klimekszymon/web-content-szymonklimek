# Reguła 60-30-10 w UX designie

Reguła 60-30-10 to prosta zasada projektowania interfejsów oparta na proporcjach kolorów, która zapewnia harmonię wizualną i czytelność.

## Podział kolorów

- **60% kolor główny** (primary color) – tło, główne powierzchnie
- **30% kolor wtórny** (secondary color) – nagłówki, przyciski, akcenty
- **10% kolor akcentowy** (accent color) – call-to-action, linki, powiadomienia

## Przykładowa paleta

| Proporcja | Kolor | Zastosowanie |
|-----------|-------|--------------|
| 60%       | #F8F9FA | Tło strony, karty |
| 30%       | #343A40 | Nagłówki H1-H3, przyciski |
| 10%       | #007BFF | CTA, linki, ikony |

## Zalety reguły

- **Hierarchia wizualna** – użytkownik wie, co jest najważniejsze
- **Spójność** – jednolity wygląd całej aplikacji
- **Czytelność** – wysoki kontrast i odpowiedni balans
- **Branding** – łatwe dopasowanie do kolorów firmy

## Praktyczne zastosowanie

```css
/* 60% - tło główne */
body { background: #F8F9FA; }

/* 30% - nagłówki i przyciski */
h1, h2, .btn-primary { background: #343A40; color: white; }

/* 10% - akcenty */
.btn-cta, a { color: #007BFF; }
```

**Reguła 60-30-10 gwarantuje profesjonalny wygląd bez skomplikowanego projektowania kolorów.**