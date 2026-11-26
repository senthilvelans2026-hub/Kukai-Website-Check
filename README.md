# Kukai 3D store

Two files run the store:

- `index.html` — the website: walking, looking, product panels, the cashier, checkout.
- `store.glb` — the whole shop as one 3D model. Open it in Blender, change anything, export it again with the same name.

Put both files side by side in the GitHub repo. GitHub Pages serves them as they are.

## Editing the store in Blender

1. **File → Import → glTF 2.0** and pick `store.glb`.
2. Everything sits in groups (empties): `ROOM`, `FIXTURES`, `CLUTTER`, `FOREGROUND`, `CEILING`, `SIGNS`, `SLOTS`, `MARKERS`, `LIGHTS`. Move, delete, add or retexture anything.
3. **File → Export → glTF 2.0**, with these settings:
   - Format: **glTF Binary (.glb)**
   - Include: tick **Custom Properties**
   - Transform: **+Y Up** (the default)
   - Name the file `store.glb`.
4. **Quick check:** drag the new `store.glb` onto the open website. It previews straight away without uploading. A note shows how many products found their slots.
5. **Publish:** replace `store.glb` in the repo and push.

Units are metres. In Blender the shop runs along **X**, the back wall is the **y = 0** plane, and the customer stands at **y = −5.2** with eyes **1.5 m** up. Anything between them and the wall is in shot. Things at **y ≈ −3.3** (the `FOREGROUND` group) slide past in front and give the depth.

## Names the website looks for

Blender adds `.001` to copies. Rename copies so every name below is unique.

| Name | What it is | What the site does |
|---|---|---|
| `hang_<id>` (empty) | A hook on a wall, e.g. `hang_p1` | Hangs that product on a card with its stock boxes behind and a price tag |
| `stand_<id>` (empty) | A spot on a shelf, e.g. `stand_w3` | Stands the product there, with copies for stock |
| `units` (custom property on a slot) | How many copies to show | 1–4; default 3 (hooks 4) |
| `spread` (custom property on a slot) | `x` = side by side, `z` = one behind the other | Default `z` |
| `sign_<key>__<style>` (flat plane) | A sign; the site writes the words on it | Styles: `neon`, `led`, `board`, `chalk`, `logo` |
| `zone_<key>` (empty) | Middle of a section | Builds the floor-plan map and the quick-jump stops |
| `cam_left`, `cam_right` (empties) | Ends of the walk | The customer walks between them, at their height and depth |
| `cashier_seat` (empty) | Where the cashier sits | Places her (or `cashier.glb`) here |
| `counter_…` (meshes) | The checkout counter | Clicking it opens the bag |
| `spot_…` / `point_…` (empties) | Lights | Spotlight aimed at the back wall / warm point light |

Section keys are `checkout`, `pleasure`, `wellness`, `massage` and `grooming`. Extra sign keys are `open`, `offer`, `end`, `counter` and `nojudge`. Change the words in `CATS` and `SIGN_TEXT` at the top of the script in `index.html`.

Slots face the customer when not rotated (glTF +Z, Blender −Y). Rotate a slot and the product turns with it.

Product ids, names, prices and stock live in `PRODUCTS` in `index.html`. A product with no slot in the model is simply not shown.

## Textures and a painted look

- Use ordinary image textures plugged into Principled BSDF → Base Color. Bake anything procedural first; node trees don't export.
- Emission exports too (neon).
- Keep the file under about 10 MB: textures at 2K or less, and Draco compression off (the site doesn't load it).

## Cashier and product models (optional)

- **`cashier.glb`:** seated pose with the stool included. Use one skeleton with a bone named `Head`, and animations named `Idle`, `Notice` and `Point`. Set its path in `MODELS.cashier`.
- **Products:** real size, origin at the centre of the base. Set paths in `MODELS.products`, e.g. `{p1:'models/p1.glb'}`.

## Running it on your laptop

Browsers won't load `store.glb` from a double-clicked file. In the folder, run `python -m http.server 8000`, then open http://localhost:8000.
