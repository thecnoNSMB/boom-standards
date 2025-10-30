Note: This is unrelated to the `levelstat.txt` output file DSDA-Doom can produce when processing a demo.

# WAD Stats (aka stats.txt)

```
1
40
E1M1 1 1 4 519 3738 -1 2 40 29 24 3 29 38 3
E1M2 1 2 0 -1 -1 -1 0 0 0 0 0 -1 -1 -1
E1M3 1 3 0 -1 -1 -1 0 0 0 0 0 -1 -1 -1
E1M4 1 4 0 -1 -1 -1 0 0 0 0 0 -1 -1 -1
E1M5 1 5 0 -1 -1 -1 0 0 0 0 0 -1 -1 -1
...etc
```

## What it is

stats.txt is the filename that DSDA-Doom and ports inspired by it use to track level completion stats for the level table, a feature that keeps track of the player's best times, completion rates, and completion difficulties for every level in a WAD. "WAD Stats" is an unofficial name this document will use to describe both features jointly, based on the source code file that defines them both, `wad_stats.c`.

Whenever a source port with a level table is launched, such as DSDA-Doom, it checks if a stats.txt file exists for the WADs you have loaded, and it creates this file if it does not exist. This file is always named "stats.txt", and its location within the "dsda_doom_data" folder (or similar) depends on the WADs you have loaded: specifically, it creates a folder for each wad name, in lower case, nested within each other according to your load order. For example, if you load `DOOM2.WAD`, then `SCYTHE.WAD`, then `scythe_ws.wad`, the stats.txt file will be at `doom2/scythe/scythe_ws/stats.txt` inside of the port's data folder. If no PWADs are loaded, or if the PWADs loaded do not contain any levels, then the stats file will contain stats for all the levels in the IWAD. However, if the PWADs loaded do contain levels, the stats file will only contain stats for those levels, and will not track any levels that weren't replaced. Wads loaded automatically from the `autoload` folder are ignored. Note that if the WAD files you load have their contents changed, or if you add or remove any WADs from the autoload folder, the stats.txt file will not be updated to compensate, so any new levels will not be tracked.

In DSDA-Doom, stats for a level are only saved if the level was played from a pistol start with monsters enabled. Generally, stats are only updated when your playthrough of a level has improved upon the previous record, but some stats have additional conditions, detailed in the table in the next section. Additionally, if a level is completed on a higher difficulty, that will override records set on a lower difficulty, except for Nightmare.

## Syntax

The stats.txt file format consists of two lines at the top of the file which apply to the entire WAD or set of WADs being described, then one line per map that is loaded.

The first line is an integer defining the syntax version used by this stats.txt file; at time of writing only version 1 is defined and used by source ports, and that is the version described here. (Cherry Doom 1.0 versions define a version 2, which is not described here, but Cherry Doom 2.0 reverted back to wad stats version 1.)

The second line is an integer that counts the total number of enemies you have killed across the entire WAD or set of WADs, including resurrected enemies and Icon of Sin spawns. Note that this is not the same thing as total kills in general, and the two can differ when playing in multiplayer. In singleplayer, all enemy kills are credited to you, regardless of how they died.

The rest of the lines track the stats for all the levels in the loaded WAD or set of WADs, one per line. In principle these lines could be in any order, but dsda-doom saves them in ascending numerical order, first by episode and then by map. These lines contain the following 15 fields, in order, separated by whitespace:

| Field | Type | Description |
|-------|------|-------------|
| Lump | Text (at most 8 characters long) | The name of the lump representing the level. |
| Episode | Integer | The episode number. In Doom 2 this is always 1. |
| Map | ^ | The map number within the episode. |
| Best skill | ^ | The highest skill that this level has been completed on. Typically counts from 1 (I'm Too Young To Die) to 5 (Nightmare!), though a few wads add a sixth on Nyan Doom. |
| Best time | ^ | The fastest time this level has been completed on any difficulty except Nightmare. |
| Best max time | ^ | The fastest time this level has been completed with all kills and secrets obtained, on any difficulty except Nightmare. |
| Best skill 5 time | ^ | The fastest time this level has been completed on Nightmare. |
| Total exits | ^ | The total number of times this level has been completed. |
| Total kills | ^ | The total number of enemies you have killed on this level, across all playthroughs. Resurrected enemies and Icon of Sin spawns *do* count for this. |
| Best kills | ^ | The highest kill count you have achieved on this level. Resurrected enemies and Icon of Sin spawns are not counted. |
| Best items | ^ | The highest item count you have achieved on this level. |
| Best secrets | ^ | The highest secret count you have achieved on this level. |
| Max kills | ^ | The total number of enemies in the level that count as kills. This is not calculated until the level is played. |
| Max items | ^ | The total number of items in the level. |
| Max secrets | ^ | The total number of secrets in the level. |

## Supported ports

DSDA-Doom introduced this feature in v0.26.0 and has retained it in all subsequent versions. All versions of Cherry Doom and Nyan Doom also support this feature. From DOOM With Love ceased active development before this feature was added, so even though it is a DSDA-Doom fork it does not support this feature.
