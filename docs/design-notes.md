# Design notes

Working notes for the Lazy Bastard challenge: what's decided, what needs testing, and what's still open.

## Design goal

A **long, hands-off game**. The player plans the settlement and places schematics; colonists do all
the work. The interesting constraint is not the opening, but everything the colony can't make for itself.

## Decided

- **Hauling and crafting are different.** Moving items is allowed; converting them is not. This mirrors
  Factorio's Lazy Bastard, which limits handcrafting, not walking around.
- **Hut blocks are colony-made.** The Sawmill learns them via datapack crafter recipes.
- **The supply camp is the whole starting inventory** (Town Hall, Build Tool, Builder's Hut + materials,
  Forester's Hut, Sawmill hut, a few beds).
- **Target version:** Minecraft 1.21.1 / NeoForge, MineColonies 1.1.1420+ (latest as of Nov 2026).
- **Victory:** all research, Town Hall L5, then survive the final raid with guards only.
- **Animals:** one breeding pair per animal hut may be led in by the player.
- **Phase 0 (scouting):** normal survival until the supply camp is placed; only personal gear carries over.
- **No free-building** after founding, anywhere. A separate outside economy would leak into the colony
  and undo the "your gear depends on the colony" part of the late game.
- **Sophisticated Backpacks:** not used.
- **Trade Post add-on:** not used. Its Marketplace Rare Finds felt overpowered for this challenge.

## Why the Sawmill (and not the Builder)?

The original idea was for the **Builder** to craft hut blocks. The Builder's Hut doesn't appear to have a
crafting module, though, so datapack crafter recipes probably can't target it without writing a mod.

The **Sawmill** is a natural fit: the Carpenter crafts items that are mostly wood, and hut blocks are
mostly planks. Datapack crafter recipes aren't bound by the Carpenter's normal "75% wood, no
ingots/stone" teaching rule, so hut recipes that include stone or iron still work.

## Datapack plan

MineColonies crafter recipes are JSON files in a datapack. Relevant properties:

| Property | Use here |
|---|---|
| `type` | `"recipe"` |
| `crafter` | The Sawmill's crafter name *(confirm from MineColonies' GitHub defaults)* |
| `inputs` | The normal hut block ingredients |
| `result` | The hut block ID *(confirm in JEI)* |
| `min-building-level` | Gates the recipe by Sawmill level |
| `research-id` | Gates late-game huts behind research |
| `show-tooltip` | `true`, so JEI/tooltip shows where the hut block comes from |

Draft shape (placeholders, not final):

```json
{
  "type": "recipe",
  "crafter": "<sawmill crafter name>",
  "inputs": [
    { "item": "minecraft:oak_planks", "count": 8 },
    { "item": "<characteristic item>", "count": 1 }
  ],
  "result": "<hut block id>",
  "count": 1,
  "min-building-level": 1,
  "show-tooltip": true
}
```

**Decision needed:** should the player's normal crafting recipes for hut blocks be **removed**
(enforces the rule) or **kept** (relies on honour)? Removing them makes the challenge self-policing.

### Draft unlock tiers

| Unlock | Hut blocks |
|---|---|
| Sawmill L1 | Warehouse, Courier, Residence, Tavern, Farmer, Fisher, Dining Hall, Chef's Kitchen, Miner, Shepherd, Fletcher |
| Sawmill L2–3 | Smeltery, Stonemason, Blacksmith, Guard Tower, Dyer |
| Research | University and late-game huts — tie each to the research that normally unlocks it |

Not yet placed: Builder's Hut (extra builders), Town Hall upgrades, Barracks, Hospital, Library, School,
Graveyard, Enchanter, Alchemist, Composter, Florist, Beekeeper, Concrete Mixer, Crusher, Sifter,
Glassblower, Mechanic, Plantation, Nether Miner, and animal huts other than the Shepherd.

## Early priorities

The **Tavern**, **Dining Hall** and **Chef's Kitchen** matter early: the Tavern brings in new
colonists, and the Dining Hall and Chef's Kitchen keep them fed. All three are in Sawmill tier 1.

To test: what each needs to run (ingredients, which raw foods the Kitchen wants), so the
Farmer/Fisher can be set up to feed it.

## Bed chain

Beds are 3 wool + 3 planks, so the Sawmill can't make them under normal rules. The **Fletcher**
(string/wool items) should be able to learn them.

Forester → Sawmill (planks) + Shepherd (wool) → Fletcher (beds) → Dyer (coloured beds, if the style needs them)

The supply camp includes a few beds to bridge the gap.

## Custom supply camp

Needs a custom style pack whose supply camp chest contains the starting inventory above.
To investigate: how to edit an existing supply camp blueprint's chest contents with the current
Structurize scan tool, and whether a style pack can ship *only* a supply camp.

## To test in-game

- [ ] Does the Sawmill or Forester require research before it can be placed?
- [ ] Exact `crafter` name for the Sawmill (check MineColonies' GitHub default crafter recipes).
- [ ] Exact hut block IDs (JEI).
- [ ] Can the Fletcher be taught the bed recipe?
- [ ] Which beds and how many do the chosen style's Builder's Hut and first Residence need?
- [ ] Does a crafter recipe with stone/iron inputs work on the Sawmill?

## Open questions

- **Loot:** allow guard-tower drops into the colony, or ban all loot?
- **Dead ends:** strict (do without) vs. a 111-item exemption budget.
- **Final raid:** do colonist deaths count against you? Confirm the raid command's syntax in your version.
- **Modpack gaps:** list any modded items in the pack that no colonist can produce.
