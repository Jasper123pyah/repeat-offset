# Repeat Check

Een mini web-app om seamless repeats van textile dessins te checken — zoals de Photoshop "Offset" filter, maar dan in de browser. Werkt op iPad als PWA (geen App Store nodig).

## Wat doet het

Je upload een herhalend dessin. De app kan:

- **Origineel** — gewoon je input tonen
- **Offset** — de afbeelding 50% (of een andere waarde) verschuiven en wrappen, zodat de repeatranden in het midden komen. Daar zie je meteen of er een kruis / naad zichtbaar is.
- **Tegels 2×2** — de offset-tegel 4 keer naast elkaar tonen, zodat je ziet hoe het dessin er als echte herhaling uit gaat zien.

X en Y offset zijn allebei los te slidieren (0–100%). Quick presets voor 50/50, 25/25, 50/0, 0/50.

Bewaar of deel het resultaat als PNG.

## Op de iPad zetten

1. Hosting (gratis): zet de folder op [Vercel](https://vercel.com), [Netlify](https://www.netlify.com) of GitHub Pages. Drag-and-drop deploy werkt prima — geen build step nodig.
2. Open de URL in Safari op de iPad.
3. Tik op **Delen → Voeg toe aan beginscherm**.
4. Open vanaf het home screen — draait full-screen als losse app, ook offline.

## Lokaal draaien

```bash
cd repeat-offset
python3 -m http.server 8000
# open http://localhost:8000
```

Of gewoon `index.html` openen in een browser, maar de service worker werkt dan niet (offline cache uit).

## Bestanden

- `index.html` — alle UI en logica in één bestand
- `manifest.webmanifest` — PWA metadata
- `sw.js` — service worker (offline cache)
- `icon.svg` / `icon-192.png` / `icon-512.png` / `apple-touch-icon.png` — app icoon

## Hoe het werkt (technisch)

De offset is een classic image-processing operatie:

```
new[x][y] = original[(x - ox) mod w][(y - oy) mod h]
```

In canvas pak ik dat door de afbeelding 4x op verschoven posities te tekenen ((ox, oy), (ox-w, oy), (ox, oy-h), (ox-w, oy-h)). Wat buiten het canvas valt wordt geclipped, wat zichtbaar is vormt samen het gewrapte resultaat.

Voor "Tegels 2×2" maak ik eerst de offset-tegel in een off-screen canvas en plak die 4x op het zichtbare canvas.

## Browser support

- Safari iPad/iOS: ✅ (PWA install + Web Share API)
- Chrome/Edge desktop: ✅
- Firefox: ✅ (Web Share alleen op mobiel)

Werkt offline na eerste bezoek (service worker cached alle assets).

---

Built voor handige textile repeat checks ✂️
