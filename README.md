# Tyrant Map Planner

Interactive hex map planning tool for the Tyrant mobile game. Plan guild hub and outpost placement with territory overlays, paint impassable terrain, and share maps with your guild via JSON export/import.

## Repo structure

```
index.html        ← the whole app (single file, no dependencies)
assets/           ← terrain tile images (optional)
  mountain.png
  snow.png
  forest.png
  water.png
  lava.png
map-data/         ← save exported JSON files here to share with guildmates
README.md
```

## Running it

Just open `index.html` in a browser — no build step, no server needed for basic use.

To use terrain images, you need a local server (or GitHub Pages) because browsers block loading local files via `file://`. The quickest way:

```bash
# Python
python3 -m http.server 8080

# Node
npx serve .
```

Then open `http://localhost:8080`.

## GitHub Pages (share with guild)

1. Push this repo to GitHub
2. Go to **Settings → Pages → Branch: main → / (root)**
3. Your map is live at `https://<you>.github.io/<repo>/`
4. Guildmates open the URL and import a shared JSON from `map-data/`

## Adding terrain images

In `index.html`, find the `TERRAIN` object near the top of the `<script>` block:

```javascript
const TERRAIN = {
  mountain: { fill: '#3a2a1a', stroke: '#5a4030', label: 'Mountain', img: null },
  snow:     { fill: '#d8eeff', stroke: '#a0c8e8', label: 'Snow peak', img: null },
  ...
};
```

Change `img: null` to a path relative to `index.html`:

```javascript
mountain: { fill: '#3a2a1a', stroke: '#5a4030', label: 'Mountain', img: 'assets/mountain.png' },
```

**Image tips:**
- Square images work best (64×64 or 128×128 px)
- Tileable textures look great at low zoom
- The image is clipped to the hex shape automatically
- The `fill` colour still shows as fallback if the image fails to load

## Controls

| Action | Desktop | Mobile |
|---|---|---|
| Pan | Drag | Single-finger drag |
| Zoom | Scroll wheel | Pinch |
| Place / paint | Click | Tap |

## Map data format

Exported JSON looks like this — safe to hand-edit:

```json
{
  "terrain": {
    "45,112": "mountain",
    "46,112": "mountain"
  },
  "hubs": [
    { "col": 129, "row": 80, "label": "Main Hub" }
  ],
  "outposts": [
    { "col": 115, "row": 95, "label": "North OP" }
  ]
}
```

## Map constants

Edit these at the top of the `<script>` block if the game patches zone sizes:

| Constant | Value | Meaning |
|---|---|---|
| `GRID` | 259 | Map is 259×259 hexes |
| `CX / CY` | 129 | Centre coordinates |
| `SANCT_R` | 40 | Sanctuary radius |
| `MID_R` | 100 | Midlands radius |
| `HUB_R` | 11 | Guild hub territory radius |
| `OUT_R` | 6 | Outpost territory radius |
