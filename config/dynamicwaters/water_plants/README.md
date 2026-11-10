# Dynamic Waters - water plants

A set of water plants says what grows in and along a biome's waters - in a list for each
kind of water:

| List | Water |
|---|---|
| `lowland` | the lowland (2D) rivers and their deltas |
| `mountain` | the mountain (3D) rivers and streams |
| `lake` | the lakes |

A list is a list of plants, tried in order: the first that grows at a place takes it.
The built-in sets have the mod's kelp in the lakes, seagrass in the mountain rivers and
the lakes, and lily pads and reeds along calm water; `warm_waters` adds a few sea pickles
in the warm biomes' lakes and slow rivers.

Needs `waterPlants = true` in `config/dynamicwaters-common.toml` (it is on by default).
Applies to newly generated chunks. Files are read again every time a world is opened.

## How to change a set

1. Copy a file from `examples/` up into this folder (the one with this README).
2. Edit the copy. Only the files in THIS folder are loaded, never `examples/`.
3. Open the world again and explore new land.

A file here named like a built-in set replaces it.

## A plant

| Field | Example | Meaning |
|---|---|---|
| `block` | `"minecraft:sea_pickle"` | The block. With its properties - `"minecraft:sea_pickle[pickles=2]"` - it is laid exactly so. |
| `at` | `"bed"` | `bed`: on the bottom, under the water. `surface`: on still water, as a lily pad. `shore`: on the bank, on the ground at the water's level, as reeds. Left out: lily pads and frogspawn on the surface, sugar cane on the shore, the rest on the bed. |
| `chance` | `0.002` | Of the places it could grow: 1 is everywhere, 0.002 one in 500. |
| `depth` | `[1, 5]` | Blocks of water over the bed, from and to. For `surface`, the water under it. |
| `calm` | `true` | Only on calm water: a lake, a river's slack margins, a delta, a mountain river where it hardly falls. |
| `patch_size` | `7` | Grows only in patches about this many blocks across... |
| `patch_share` | `0.3` | ...that cover this share of the water. `chance` is then how thick a patch is. |
| `alone` | `true` | Never next to another of it. |
| `height` | `[1, 3]` | How many blocks tall, for a plant that stacks: reeds. |
| `salt` | `1234` | Any number: it shuffles where the plant grows. |

A plant can be just its block id - `"minecraft:seagrass"` - with everything else as
the table's defaults: on its usual place, `chance` 0.05, any depth.

Kelp grows in a stalk from the bed up toward the surface; tall seagrass takes two blocks
of water; sea pickles come one to four to a block. A block that can hold water is laid
holding it, under the water. Nothing grows on the surface or the shore where it snows:
the water freezes over there.

## Which set a biome gets

The same as for the riverbank palettes, your files ahead of the built-in ones:

| Field | Example | Meaning |
|---|---|---|
| `where.biomes` | `["minecraft:jungle"]` | Use this set in these biomes. |
| `where.tags` | `["c:is_hot/overworld"]` | ...or in a biome with ANY of these tags. |
| `where.climate` | `{"temperature": [1.0, 2.5], "downfall": [0.0, 0.3]}` | ...or in a biome whose climate falls in this window. |
| `where.not` | `["minecraft:stony_peaks", "#c:is_snowy"]` | Never in these biomes, nor in a biome with these `#tags`. |
| `enabled` | `false` | Nothing grows wherever this set applies. |

1. A set that names the biome in `where.biomes`.
2. Otherwise the first one with one of the biome's tags in `where.tags`.
3. Otherwise one whose `where.climate` the biome fits - the narrowest window.
4. Otherwise the one that names nothing - the default.

ONE set per biome: a set for some biomes has to list the default set's plants too, or
its waters lose them. A lowland river runs in the `minecraft:river` biome, but it takes
the set of the land around it.

## Example

Sea pickles here and there in the lakes of the jungles, the kelp kept:

```json
{ "where": { "tags": ["c:is_jungle"] },
  "lake": [
    { "block": "minecraft:kelp", "depth": [4, 64], "patch_size": 7, "patch_share": 0.3, "chance": 0.7, "salt": 27441 },
    { "block": "minecraft:sea_pickle", "calm": true, "depth": [1, 6], "chance": 0.004 }
  ] }
```
