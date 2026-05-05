# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Projekt

Statisk personlig landningssida för Johan Lindengard (projektledare). Sidan är på svenska och består av en enda sida: hero, citat, fyrstegsmetod (`approach`), interaktiva scenarier (`<details>`), längre rekommendationer och kontaktsektion.

## Stack och utveckling

Ren HTML + CSS — **ingen build, inga tester, inga dependencies, ingen `package.json`**. Filerna i roten: `index.html`, `styles.css`, `CNAME`, `README.md`.

Lokal förhandsgranskning: öppna `index.html` direkt i webbläsaren, eller starta en enkel server från projektroten:

```sh
python3 -m http.server 8000
# → http://localhost:8000
```

## Deploy

Push till `main` triggar `.github/workflows/deploy-pages.yml`, som publicerar hela repo-roten till GitHub Pages. Det finns inget separat dist/build-steg — det som ligger i roten är det som går live. Om en ny long-lived branch behövs i deploy-flödet måste den läggas till i workflowens `branches`-lista.

Live på **https://whois.johanlindengard.com** (HTTP redirectas till HTTPS, certifikatet förnyas automatiskt av GitHub).

### Custom domain

`CNAME`-filen i roten innehåller `whois.johanlindengard.com` och **måste finnas där**. Workflowen laddar upp hela repo-roten som artifact (`build_type: workflow`); utan CNAME-filen i artifacten tappar GitHub Pages custom domain-inställningen vid varje deploy och Let's Encrypt slutar utfärda HTTPS-cert. Ändra inte filens innehåll utan att också uppdatera DNS hos domänleverantören och custom domain-fältet i Settings → Pages.

DNS hos domänleverantören: `whois.johanlindengard.com` är en `CNAME` som pekar på `cola500.github.io`.

## Designkonventioner att respektera

Att förstå genom att läsa `styles.css` ensam tar tid — här är de val som styr utseendet:

- **Mörk varm palett**: bakgrund `#1c1815`, text `#ede4d3`, accent `#f6efde`. Färgvärden är hårdkodade (inga CSS-variabler) — håll paletten konsekvent vid ändringar.
- **Typografi**: rubriker (`h1`–`h3`) använder serif (Georgia-stack), brödtext system-sans. `clamp()` används flitigt för responsiv typografi — föredra det före media queries för storlek.
- **Layout-brytpunkt**: `max-width: 700px` är den enda brytpunkten som används genomgående. Mobile-first regler först, desktop-overrides i `@media (min-width: 701px)` blocks senare i filen.
- **Sektionsrytm**: varje `<section>` har `margin-bottom: 8rem` (6rem på mobil). Behåll luften — sidan är medvetet luftig.
- **Accessibility**: varje sektion har `aria-labelledby` kopplad till sin rubrik, och `<details name="scenarios">` används för att bara ett scenario ska kunna vara öppet åt gången. Bevara dessa mönster vid nya sektioner.
- **Inga JS-beroenden**: interaktiviteten (utfällbara scenarier) bygger på native `<details>`/`<summary>`. Lägg inte till JavaScript utan stark anledning.

## Innehållsriktlinjer

Texten på sidan är medvetet kort, personlig och svenskspråkig — tonen är "lugn självsäkerhet, inte säljig". Vid copy-ändringar: behåll korta meningar, undvik buzzwords, och håll personliga formuleringar (jag-form). Se commit-historiken för tidigare "tightening passes" som referens på tonen.
