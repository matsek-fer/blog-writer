# blog-writer

Alat Matematičke sekcije FER za pisanje **blogova za knjižnicu**: vodi te
od ideje do gotovog, validiranog bundlea — intervju o temi i publici,
istraživanje postojećih materijala, struktura po odjeljcima (svaki uči
točno jedan koncept), checkpointi, engleska anotacija za pretraživanje i
tutor-stub za nastavak učenja u svom tutoru.

## Instalacija

Dvije naredbe u Claude Codeu:

```
/plugin marketplace add matsek-fer/plugins
/plugin install blog-writer@matsek
```

Nakon toga samo reci da želiš napisati blog za knjižnicu — vještina se
aktivira sama. Ažuriranja stižu automatski kroz marketplace.

## Kako se uklapa u ekosustav

| Alat | Uloga |
|---|---|
| [library](https://github.com/matsek-fer/library) | Zajednička knjižnica bundleova (zadaci, dokazi, blogovi) — **odredište** onoga što blog-writer proizvede. Objavljena na <https://matsek-fer.github.io/library/>. |
| **blog-writer** (ovaj repo) | Autorski protokol: piše blog kao bundle po [spec-u](https://github.com/matsek-fer/spec), validira ga do nule grešaka i vodi PR prema knjižnici. |
| tutor (`AI_instructor`) | Svaki blog nosi `tutor-stub.json`: preuzmeš ga u svoj vault i `/tutor` nastavlja točno ondje gdje je blog stao. |
| problemset | Pretraživanje knjižnice — blog-writer ga koristi obrnuto: pronalazi zadatke vrijedne linkanja iz odjeljaka bloga. |

## Forest-readiness

Blogovi se pišu kao **budući objekti Knowledge Foresta**: svaki odjeljak
je samostalno čitljiv, nosi jedan koncept i klasificiran je taksonom
(`exposition`, `example`, `intuition`, `motivation`, `connection`) u
`x_forest` polju frontmattera. Normativna definicija konvencije:
[`docs/forest-readiness.md`](docs/forest-readiness.md) (na engleskom —
model-facing dokument, kandidat za promociju u spec format v2).

## Napomena o provenijenciji

Sadržaj koji je model skicirao nosi `provenance: ai-assisted` i **čeka
pregled maintainera** prije nego što se pojavi u knjižnici. Prepisivanje
ili blisko parafraziranje udžbenika, natjecateljskih zadataka i
math.StackExchangea zabranjeno je politikom
[`spec/policies/provenance.md`](https://github.com/matsek-fer/spec/blob/main/policies/provenance.md).
