# Lookup — this checkout first

**Shared fact.** Both Cloud agents. Do not tile-ring an NPC you have not
looked up.

Order:

1. `wiki/npcs/<name>.md` coordinate table (this dump, not OSRS Wiki).
2. `server/content/maps/mXX_YY.jm2` `==== NPC ====`. Line
   `level localX localZ: id` → world
   `( (mx<<6)+localX, (mz<<6)+localZ )`.
   Example: `m50_49.jm2` `0 35 17: 458` → Urhney **`(3235, 3153)`**.
3. The quest `.rs2` (Aereck even says Urhney is in the **eastern** swamp).
4. OSRS memory last. It is often wrong here.

`findNearbyLoc` is a short window (~6 tiles). `await sdk.scanNearbyLocs(15)`
is wider. Neither finds an NPC 80 tiles away. "West/north blocked, no door"
on the looked-up tile means you are on a **different building**.

Do not write a discovery-ring script. Walk to the sourced tile, scan, talk.

Waterfall Golrie: surface climb-down is `loc_1754` at **`(2533, 3155)`**
(`m39_49.jm2` `0 37 19: 1754`). That loc is `+6400` z
([`ladders.rs2`](../server/content/scripts/ladders+stairs/scripts/ladders.rs2)).
Golrie himself is `(2515, 9581)` (`m39_149` `0 19 45: 306`). Elkoy
(`elkoy.rs2`) only leads the maze after Tree Gnome Village has **started** —
the intro is flavor, not an escort.
