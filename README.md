# Pavel Filandr — osobní web

Statický jednostránkový web. Žádný build, žádné závislosti, žádný node_modules.
Nahraješ na hosting a běží.

---

## Struktura

```
.
├── index.html          ← web (86 kB), tenhle soubor se nasazuje
├── assets/             ← 16 obrázků ve WebP (≈ 365 kB dohromady)
├── offline/
│   └── index.html      ← stejný web zabalený do jednoho souboru (850 kB)
├── .nojekyll           ← vypíná Jekyll na GitHub Pages
└── README.md
```

**`index.html`** tahá obrázky z `assets/`, fonty z Google Fonts a GSAP + Lenis z CDN.
Tohle nasazuj na web.

**`offline/index.html`** má všechno uvnitř – obrázky jako data URI, fonty jako
subsetované woff2, knihovny vložené inline. Nepotřebuje internet ani složku
`assets/`. Hodí se na posílání e-mailem, na prezentaci u klienta nebo když si to
chceš jen otevřít dvojklikem z plochy.

---

## Spuštění lokálně

Dvojklik na `index.html` stačí. Pokud chceš server (kvůli relativním cestám v
některých prohlížečích):

```bash
python3 -m http.server 8000
# http://localhost:8000
```

---

## Nasazení na GitHub Pages

1. Nahraj obsah repozitáře do větve `main`.
2. **Settings → Pages → Source:** `Deploy from a branch`, větev `main`, složka `/ (root)`.
3. Za pár minut poběží na `https://<uživatel>.github.io/<repo>/`.
4. Vlastní doména: **Settings → Pages → Custom domain**, u registrátora nastav
   `CNAME` na `<uživatel>.github.io`.

Alternativně Netlify nebo Vercel – přetáhni složku do okna, hotovo.

---

## Co ještě čeká na doplnění

| Kde | Co | Jak najít v kódu |
|---|---|---|
| Kontaktní formulář | odeslání na e-mail | hledej `TODO: endpoint` |
| Hlavička | `og:image` na absolutní URL | hledej `og:image` |

### Formulář

Validace běží v prohlížeči, ale data se nikam neodesílají. Nejrychlejší cesta je
[Formspree](https://formspree.io) nebo [Netlify Forms](https://docs.netlify.com/forms/setup/).
V `index.html` najdi komentář `TODO: endpoint` a doplň `fetch()` na svůj endpoint.

Jakmile formulář poběží, patří na web **cookie lišta a informace o zpracování
osobních údajů** (GDPR).

---

## Technicky

- **Fonty:** Big Shoulders Display (nadpisy), Montserrat (text), Newsreader italic
  (jediná citace v sekci Onkolehko). V offline verzi jsou subsetované jen na znaky, které web používá.
- **Knihovny:** GSAP 3.12.5 + ScrollTrigger (animace), Lenis 1.1.14 (plynulý scroll).
  Pokud se CDN nenačte, web funguje dál – jen bez animací.
- **Obrázky:** WebP, `loading="lazy"` kromě hero portrétu.
- **Přístupnost:** sémantické HTML, `aria-labelledby` u sekcí, zlatý focus ring,
  plná podpora `prefers-reduced-motion` (vypne animace i preloader).
- **Responzivita:** breakpointy 1080 / 820 / 640 / 480 px, testováno 360 až 2560 px. Na mobilu a tabletu
  na výšku je hero skládané pod sebe (portrét nahoře, jméno najíždí přes jeho spodní hranu),
  aby text nepřekrýval obličej.
- **Preloader** běží jednou za relaci (`sessionStorage`): podpis se dopíše, přiblíží se
  za okraje obrazovky a naskočí úvodní stránka. Pojistka ho po 4,5 s odemkne,
  i kdyby cokoli selhalo. Podpis pro intro je ve vyšším rozlišení (`signature-lg.webp`).
- **Video** v sekci Média je rozhovor z A11 TV (přehrávač Bunny Stream), načte se
  až po kliknutí. Odkaz na embed je v atributu `data-embed`.

### Úprava barev a typografie

Všechno je v CSS proměnných na začátku souboru, v bloku `:root`:

```css
--gold:   #C6A15B;   /* akcent */
--bone:   #F4F1EC;   /* hlavní text */
--muted:  #9A948B;   /* sekundární text */
--gut:    clamp(1.5rem, 6vw, 7rem);   /* odsazení od kraje, drží navbar i sekce */
```

---

## Obsah a práva

Fotografie, loga a texty jsou majetkem Pavla Filandra a příslušných značek
(Vitalium, Onkolehko, Filandr Wear, Ostříháno, Drsný stylista). Kód webu je
zakázková práce pro tento projekt.
