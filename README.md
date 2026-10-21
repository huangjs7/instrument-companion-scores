# Instrument Companion score catalog

The [catalog](https://huangjs7.github.io/instrument-companion-scores/catalog.json) combines **4 curated modern piano scores**, **1,948 Mutopia PDF links** and **3 original practice exercises** (1,955 entries). The app caches the directory, searches titles/composers/explicit aliases, downloads selected PDFs and reads them offline. External PDFs stay on their original hosts; they are not mirrored here.

## Familiar modern pieces (added 2026-10-22)

| Search title | Composer | Source level | PDF pages |
| --- | --- | --- | --- |
| Titanic / 泰坦尼克号 / 我心永恒 | James Horner | Easy | 2 |
| Pirates of the Caribbean / 加勒比海盗 | Klaus Badelt | Easy–Intermediate | 2 |
| River Flows in You / 你的心河 | Yiruma | Easier piano arrangement | 3 |
| Comptine d'un autre été / 天使爱美丽 | Yann Tiersen | Easy–Intermediate | 2 |

These are the original piano arrangements by **Hetty Sponselee**, publicly offered on [Piano Tunes' Free Sheet Music page](https://www.pianotunes.eu/classic-pop.php). The directory records that public download availability, not an open licence or permission to republish. All composition/arrangement rights remain with their respective holders. Only metadata and original-host links are published here; no external score is copied, altered or relicensed. None has an invented recommended key map. They are simplified piano scores, not a claim of one-finger or 16-key playability. Search `现代` for these four entries.

## Mutopia source and licensing

The [Mutopia Project](https://www.mutopiaproject.org/) publishes freely downloadable music under per-score public-domain or Creative Commons terms. See its [source repository](https://github.com/MutopiaProject/MutopiaProject) and [license details](https://www.mutopiaproject.org/legal.html).

Each imported entry preserves title, composer, instrument where supplied, typesetter and edition credit, original PDF URL, upstream revision, source page, license name and link. Its terms remain those of the upstream score. The CC0 dedication for our original exercises does **not** apply to Mutopia material. No scores were transposed, arranged or converted during import. The app exposes each score's source and licensing information.

The computer-side importer reads the official website's `datafiles/musiccache.dat`, using the record layout in its open [website listing code](https://github.com/MutopiaProject/MutopiaWeb/blob/master/cgibin/make-table.cgi). This cache is not a documented public API. Malformed or duplicate records stop the import. The phone reads only our static JSON, without parsing third-party pages.

The current snapshot contains 2,124 entries: 1,948 direct PDFs and 176 archive entries excluded from this PDF-only reader. [mutopia-import.json](mutopia-import.json) records the source hash and exclusions. Inclusion does not mean a piece is adapted to the 16-key instrument. Imported entries have no invented recommended notes.

## Catalog contract

`schemaVersion: 1` and `scores[]` remain. Required: `id`, `title`, `source`, `license`, `pdf`, `revision`. Stable IDs use lowercase letters, digits and hyphens. PDF paths resolve relative to the catalog and must use HTTPS.

Optional: `composer`, `instrument`, `aliases` (explicit nonempty strings), `sourceUrl`, `licenseUrl`, `notes` (exactly 16 integer MIDI notes, 0..127). Omit `notes` for unadapted scores. Explicit invalid values, including null, are rejected. Original entries and old download snapshots remain readable. Applying a recommendation always requires a user choice.

Search matches title, composer and explicit aliases. Aliases cover selected Chinese composer names and a few verified titles; they are not automatic translations of the whole library. At most 30 matches render per batch. The catalog is about 1.06 MB, below the app's 2 MiB limit.

## Original exercises

| Score | Meter | Quarter-note tempo |
| --- | --- | --- |
| [C Major Steps](scores/c-major-steps.pdf) | 4/4 | 80 |
| [Three-Beat Arc](scores/three-beat-arc.pdf) | 3/4 | 72 |
| [Open Sky Arpeggios](scores/open-sky-arpeggios.pdf) | 4/4 | 76 |

Each has eight complete bars of numbered notation. [exercises.json](exercises.json) preserves the `[MIDI note, quarter-note beats]` events. Recommended map: `60 62 64 65 67 69 71 72 74 76 77 79 81 83 84 86` (C4..D6).

These original exercises were generated with AI assistance on 2026-10-21, specifically for this project, without copying any named composition or external engraving. No claim of human authorship of AI-generated material is made. To the extent applicable rights exist, the **three original exercise melodies, source data and generated layouts** are dedicated under [CC0 1.0 Universal](https://creativecommons.org/publicdomain/zero/1.0/legalcode.en). This does not cover third-party scores, software or standard fonts.

## Maintainer workflow

From the parent Android project, with Python 3:

```text
python -m unittest discover -s tools -p test_import_mutopia.py
python tools/import_mutopia.py
python content/validate_content.py
powershell -NoProfile -File tools/publish_catalog.ps1
```

The importer uses only the Python standard library. `--cache build/mutopia/musiccache.dat` reuses an explicit saved snapshot. The content validator requires pypdf. Rebuilding originals with `content/build_scores.py` requires ReportLab and preserves imported entries. Publication uploads only catalog material and three original PDFs; APP/FPGA sources are not published.

The local SHA-256 manifest covers catalog.json and three original PDFs. The app checks downloaded documents with Android PdfRenderer; it does not claim upstream PDF hashes or automatic keymap generation.
