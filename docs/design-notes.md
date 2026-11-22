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
- **Hut blocks and quests, combined:** colonists make hut blocks (Sawmill datapack); a quest pack
  covers what colonists can't: animals, dead-end items and bootstrap items.
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

## Quest pack

A quest pack (most likely **FTB Quests**) works alongside the datapack. The datapack keeps the core
idea, colonists make everything they can, and quests fill the gaps.

### What quests reward

| Gap | Quest reward |
|---|---|
| Animals (colonists can't catch them) | Spawn eggs for a breeding pair when the matching hut is built, e.g. "Build a Shepherd's Hut" → 2 sheep eggs. Replaces the "lead in one breeding pair" rule. |
| Dead-end items (no colonist source) | One-off rare items for real milestones. Replaces the free 111-exemption budget. |
| Bootstrap items | Extra beds, and possibly the Forester and Sawmill hut blocks. If quests can deliver all of these, the custom supply camp style pack may not be needed. |
| Learning the rules | A first chapter that explains the challenge in-game. |

### Guardrails

- **Reward colony milestones, not player actions.** Prefer advancement tasks: MineColonies has its own
  advancements for building and upgrading huts, so most quests can complete themselves. Item-submission
  tasks are fine if the items are colony-made (handing them in counts as hauling).
- **Reward unlocks, not resources.** Spawn eggs, a hut block, a single rare item: yes. Bulk materials
  (a stack of iron): no. Bulk rewards skip the production chain.
- **Hut blocks stay colony-made.** Quests don't hand out hut blocks beyond the bootstrap, so the Sawmill
  stays the tech tree.

### Chapter outline (draft)

1. **The Charter** — rules of the challenge; scouting; found the colony.
2. **Breaking Ground** — Builder's Hut, Town Hall, Forester, Sawmill.
3. **A Working Town** — Warehouse, Courier, Tavern, food chain (Dining Hall, Chef's Kitchen).
4. **Hearth and Home** — Shepherd, Fletcher, beds; animal huts (egg rewards).
5. **Industry** — Stonemason, Smeltery, Blacksmith; Sawmill tier 2–3.
6. **Learning** — University and research milestones.
7. **The Capital** — Town Hall level 5, all research.
8. **The Last Stand** — survive the final raid.

### To check

- [ ] FTB Quests version for Minecraft 1.21.1 / NeoForge.
- [ ] Which MineColonies advancements exist, and whether they're enough to track hut levels and research.
- [ ] Whether spawn egg rewards spawn animals somewhere sensible (near the hut, not on the player).

## Custom supply camp

Needs a custom style pack whose supply camp chest contains the starting inventory above.
*May be unnecessary if quest rewards can deliver the bootstrap items — see Quest pack.*
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
- **Dead ends:** handled by quest rewards (see Quest pack). Which items, and at which milestones?
- **Final raid:** do colonist deaths count against you? Confirm the raid command's syntax in your version.
- **Modpack gaps:** list any modded items in the pack that no colonist can produce.
