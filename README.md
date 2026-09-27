# 🛡️ Armor Trim Editor

🎨 **Armor Trim Editor** is a [Blockbench](https://www.blockbench.net) plugin for painting Minecraft: Java Edition
**armor trims**. Instead of juggling two PNG files on a hand-made model, you paint directly on the exact armor model
the game uses, see every trim material while you paint, pose the player, swap skins, generate the smithing
template icon and export everything into your resource pack in one click.

<p align="center">
  <img src="docs/hero.png" alt="Armor Trim Editor in Blockbench" width="100%">
</p>

## 📋 Features

- 🧱 **Exact armor model**, taken from the game client: helmet (1.0 plus the outer 1.5 layer), chestplate (1.0),
  leggings (0.5 / 0.4, separate `humanoid_leggings` texture) and boots (0.9), with the mirrored left arm and leg.
- 🖌 **Paint only the trim.** Skin and reference armor are locked, so the brush always lands on the trim texture.
- 🌈 **Live material preview**: quartz, iron, netherite, redstone, copper, gold, emerald, diamond, lapis, amethyst
  and resin, including the `_darker` palettes used when the trim matches the armor (gold on gold, etc.).
- 🔍 **Palette highlight**: shows which pixels follow the trim material and which keep a fixed color.
- 🛡 **Reference armor** under the trim: leather (with dye color), chainmail, iron, copper, gold, diamond,
  netherite, turtle shell or none.
- 🤸 **18 poses and animations** ported from the game's `HumanoidModel`: walking, sprinting, sneaking, riding,
  attack, bow, crossbow, shield, trident, spyglass, swimming, elytra, T-pose and more, plus head yaw/pitch sliders.
- 🧍 **Skins**: all 9 default skins (wide and slim), any PNG file (legacy 64×32 skins are converted) or a
  player name.
- 🏷 **Icon generator** for the smithing template item: recolor a vanilla template (or your own icon) with a
  body and glyph color, or sample the colors from the trim.
- ✅ **Checks** before export: pixels outside the UV layout, semi-transparent pixels (trims render as cutout),
  colors that are almost but not exactly on the palette — each with a one-click fix.
- 📦 **Export to a resource pack**: both trim textures, the icon texture and model, and registration in
  `atlases/armor_trims.json` (keeps the file's indentation and path style, and adds missing vanilla palettes such as
  `copper_darker`).
- 📂 **Import** any trim from a resource pack to keep editing it; HD trims (128×64 and up) are supported.
- 🌍 English and Russian interface.

## 🌈 Material preview

Only eight gray shades are recolored by the trim material. Everything else is drawn exactly as painted, so a
trim can mix material-colored parts with fixed colors. The palette panel shows what each shade turns into
with the selected material.

<p align="center"><img src="docs/materials.png" alt="The same trim with different materials" width="100%"></p>

| Palette key | `#E0E0E0` | `#C0C0C0` | `#A0A0A0` | `#808080` | `#606060` | `#404040` | `#202020` | `#000000` |
|---|---|---|---|---|---|---|---|---|

The preview is done in the texture shader, so it updates while you paint. The textures you export always keep
the original gray palette.

## 🤸 Poses and skins

Poses are preview-only: they move the model, never the textures, and animations pause while you paint.

<p align="center"><img src="docs/poses.png" alt="Poses" width="100%"></p>
<p align="center"><img src="docs/skins.png" alt="Default skins and the skin tab" width="100%"></p>

## 🏷 Template icon

<p align="center"><img src="docs/icon_generator.png" alt="Icon generator" width="60%"></p>

The generator separates the glyph (the cyan pixels of vanilla templates) from the tablet and recolors both.
Pick a vanilla template, the current icon or an icon from your pack as the base, then touch it up by hand —
it is an ordinary 16×16 texture named `icon`.

## 📦 Export and import

<p align="center"><img src="docs/export.png" alt="Export dialog and result" width="100%"></p>

With trim ID `ember` and the default settings, an export writes:

| File | What it is |
|---|---|
| `assets/minecraft/textures/trims/entity/humanoid/ember.png` | helmet, chestplate and boots |
| `assets/minecraft/textures/trims/entity/humanoid_leggings/ember.png` | leggings |
| `assets/minecraft/textures/item/ember_armor_trim_smithing_template.png` | template icon |
| `assets/minecraft/models/item/ember_armor_trim_smithing_template.json` | icon model (`item/generated`) |
| `assets/minecraft/atlases/armor_trims.json` | adds both textures to the trim atlas |

The texture namespace, icon paths (`{id}` is replaced with the trim ID) and model parent are configurable, and
every setting is remembered. Overwritten files are backed up to the Blockbench data folder. After the first
export, **Quick export** (`Ctrl + Alt + E`) repeats it without the dialog.

The result dialog can copy an item model entry (for a `range_dispatch` on `custom_model_data`) and a
`trim_pattern` JSON for your datapack. The plugin does not touch item definitions or datapacks by itself.

<p align="center"><img src="docs/import.png" alt="Import dialog" width="50%"></p>

## 🧩 How armor trims work

A trim is drawn on the same model as the armor piece it sits on, on top of it:

| Texture | Area (u, v) | Model part | Inflation | Worn with |
|---|---|---|---|---|
| `humanoid` | 0, 0 | head | 1.0 | helmet |
| `humanoid` | 32, 0 | head, outer layer | 1.5 | helmet |
| `humanoid` | 16, 16 | body | 1.0 | chestplate |
| `humanoid` | 40, 16 | arms | 1.0 | chestplate |
| `humanoid` | 0, 16 | legs | 0.9 | boots |
| `humanoid_leggings` | 16, 16 | body (waist) | 0.5 | leggings |
| `humanoid_leggings` | 0, 16 | legs | 0.4 | leggings |

- 🪖 **Two helmet layers.** The left half of the top row is the helmet itself; the right half is an outer layer
  that floats 0.5 px above it. Toggle them separately in the *View* tab.
- 🪞 **Left and right limbs share one area**, mirrored, so they cannot differ.
- 🎨 **Palette.** Only the eight grays above are recolored. A material that matches the armor (gold trim on gold
  armor) uses its `_darker` palette.
- 🔲 **Transparency.** Trims render as cutout: alpha below 10% disappears, anything else becomes fully opaque.
- 🖥 **Server side.** The pattern itself is registered by a datapack (`data/<namespace>/trim_pattern/<id>.json`
  with `asset_id` = `<namespace>:<id>`). `"decal": true` draws the trim only over armor pixels.

## 🚀 Installation

Requires the **desktop** version of Blockbench **5.0 or newer**.

1. Download [`armor_trim_editor.js`](armor_trim_editor/armor_trim_editor.js).
2. In Blockbench open **File → Plugins**, click the *Load plugin from file* icon and pick the file.
3. When asked, allow file access — the plugin needs it to read the Minecraft jar and to write into your pack.

### Minecraft jar

Default skins, reference armor, vanilla trims and template icons are read at runtime from your own Minecraft
client jar; nothing from the game is bundled with the plugin. The jar is found automatically in:

- the official launcher (`.minecraft/versions`),
- Prism Launcher, PolyMC and MultiMC (including portable installs in common folders),
- jars downloaded by other Blockbench plugins.

Choose a different one in **Trim → Settings**. Without a jar the plugin still works with a plain mannequin
skin and flat-colored reference armor.

## 🛠 Usage

1. **File → New → Armor Trim** (or **Trim → New trim**). Start empty or from a vanilla pattern.
   To edit an existing trim use **Trim → Open trim from resource pack**.
2. Paint in the 3D view or on the `humanoid` / `humanoid_leggings` textures in the 2D editor. Use the palette
   panel for material-colored pixels. Right-click an armor piece in the *View* tab to show only that piece —
   useful for leggings, which sit under the chestplate and boots.
3. Check the result with different materials, armor, poses and skins.
4. Make the template icon with **Icon**.
5. **Export**, then press `F3 + T` in game.

The `.bbmodel` file keeps everything — textures, preview settings and export settings.

## 🌍 Localization

- 🇺🇸 **English** — default
- 🇷🇺 **Russian** — full translation

The interface follows the Blockbench language. More languages can be added in `getTranslations()` at the end
of the plugin file.

## 📄 License

[GPL-3.0](LICENSE). Not affiliated with Mojang or Microsoft. Minecraft assets stay in your own game files.
