# Dynamic Waters - riverbank palettes

A palette is what a river (lowland or mountain) or a lake lays on the ground it cut. It
does not paint bands of height: it sorts the ground by what the water does there.

| Place | Where | Typically |
|---|---|---|
| `rapids` | the bed where the water runs hard: falls, mountain streams, a river's deep line | pebbles, cobbles, boulders |
| `silt` | the bed where the water slackens: lakes, deep pools, a river's margins, delta channels | sand, clay, mud |
| `shore` | the wet margin, the first block or two over the water | pebbles, sand, roots, mud |
| `slope` | cut ground above the shore (the built-in palettes leave it as it is) | rock |
| `bars` | the shelves inside the bends where they break the surface | sand, gravel |
| `flats` | a delta's low land between its channels | sand, mud, grass |

How hard the water runs is worked out from the river itself: a mountain stream, a step
in the water, the middle of a river run hard; a lake, a deep pool, the edges of a river,
a delta's channels lie slack. The faster the water, the more of the bed is `rapids`.

Nothing is laid block by block, which reads as a spray: the commonest block of a list is
the ground, and every other lies on it in rounded spots - a spread of sand, a bed of
pebbles, a hollow of mud - the way water sorts what it carries.

Needs `riverbankPalettes = true` in `config/dynamicwaters-common.toml` (it is on by default).
Applies to newly generated chunks. Files are read again every time a world is opened.

## How to change a palette

1. Copy a file from `examples/` up into this folder (the one with this README).
2. Edit the copy. Only the files in THIS folder are loaded, never `examples/`.
3. Open the world again and explore new land.

A file here named like a built-in palette replaces it, so a copy of
`examples/broadleaf_forest.json` edited here is the forest palette from then on.

## Fields (all optional)

| Field | Example | Meaning |
|---|---|---|
| `where.biomes` | `["minecraft:forest"]` | Use this palette in these biomes. |
| `where.tags` | `["c:is_forest", "c:is_taiga"]` | ...or in a biome with ANY of these tags. |
| `where.climate` | `{"temperature": [1.0, 2.5], "downfall": [0.0, 0.3]}` | ...or in a biome whose climate falls in this window. |
| `where.not` | `["minecraft:dark_forest", "#c:is_swamp"]` | Never in these biomes, nor in a biome with these `#tags`. |
| `enabled` | `false` | Leave the cut ground as it is wherever this palette applies. |
| `rapids`, `silt`, `shore`, `slope`, `bars`, `flats` | `["minecraft:gravel*3", "keep"]` | The blocks of each place (see above). |
| `patch_size` | `5` | How wide a spot is, in blocks (1 = block by block). The beds of fast water break finer. |
| `shore_height` | `1` | How many blocks over the water the shore reaches, give or take one. |

A place that is not given keeps its ground - but for `bars`, which are made of the
palette's `silt` when not given, and `flats`, made of its `bars`.

## Mixing blocks

`*N` gives a block N shares of the ground. The block with the most shares is the ground,
the others lie on it in spots, each covering as much as its shares say:

```json
"rapids": ["minecraft:gravel*4", "minecraft:cobblestone*2", "minecraft:mossy_cobblestone"]
```

is a gravel bed with spots of cobblestone (2 in 7) and of mossy cobblestone (1 in 7).
`keep` in a list keeps the ground for its shares - `["keep*2", "minecraft:mud"]` leaves
the ground as it was with spots of mud on a third of it.

## Which palette a biome gets

One palette per biome, asked for in this order, your files ahead of the built-in ones:

1. A palette that names the biome in `where.biomes`.
2. Otherwise the first one with one of the biome's tags in `where.tags`.
3. Otherwise one whose `where.climate` the biome fits - the narrowest window.
4. Otherwise the one that names nothing - the default.

A lowland river runs in the `minecraft:river` biome, but it is dressed in the palette of
the land around it - a forest river in the forest one, a desert river in the desert one.

## Examples

Boulder-strewn streams in every taiga that is not snowy:

```json
{ "where": { "tags": ["c:is_taiga"], "not": ["#c:is_snowy"] },
  "rapids": ["minecraft:cobblestone*3", "minecraft:mossy_cobblestone*2", "minecraft:gravel"],
  "silt": ["minecraft:gravel*2", "minecraft:sand"],
  "shore": ["keep*2", "minecraft:podzol", "minecraft:mossy_cobblestone"],
  "patch_size": 4 }
```

Red mud and clay in any hot, wet biome a mod adds:

```json
{ "where": { "climate": { "temperature": [0.9, 2.0], "downfall": [0.8, 1.0] } },
  "silt": ["minecraft:mud*3", "minecraft:clay"], "shore": ["minecraft:mud", "keep"] }
```

Leave swamp water as it is:

```json
{ "where": { "biomes": ["minecraft:swamp"] }, "enabled": false }
```
