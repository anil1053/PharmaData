# PharmaData

Published data for the **Pharma Near You** app. This repository is the app's entire backend.

There is no server. The app downloads one file for the country it is in, keeps a copy on the device
and answers every query — nearest pharmacies, open or closed right now, search by town — from that
copy. That is why it works with no signal, costs nothing to run, and cannot be down when someone
needs a pharmacy.

The app's source code is in a separate, private repository. This one is public because GitHub Pages
cannot serve from a private repository on the free plan, and because there is nothing here that is
not already public: the data is OpenStreetMap's.

## What is here

| File | Contents |
|---|---|
| `dist/index.json.gz` | Which countries exist, how many pharmacies each has, when the set was built |
| `dist/pharmacies-XX.json.gz` | One country, by ISO 3166-1 alpha-2 code |
| `dist/privacy.html` | The app's privacy policy, linked from its Play Store listing |

Served at `https://anil1053.github.io/PharmaData/` — the contents of `dist/` land at the site root,
so a country file is at `https://anil1053.github.io/PharmaData/pharmacies-BG.json.gz`.

Gzipped in the repository rather than left to the host: GitHub Pages does not compress a `.json` it
is serving, and this data is about five times smaller compressed. The app asks for the `.gz` and
decompresses it itself, so nothing depends on what the host does with `Content-Encoding`.

One file per country, not one file for Europe: the full set is over 150,000 pharmacies, and nobody
needs the ones in a country they are not standing in. Bulgaria is 88 KB; the largest, France, is
1.3 MB.

## Updating

Generated from the app repository, which holds the OpenStreetMap importer and the export tool:

```
dotnet run --project backend/tools/PharmacyFinder.DataExport -- --out ./dist
```

Copy the result into `dist/` here and push. The workflow publishes it to Pages, and apps pick up the
new data the next time they check — no app update, no store review.

## Licence

Pharmacy data © OpenStreetMap contributors, licensed under the
[Open Database License](https://opendatacommons.org/licenses/odbl/).
