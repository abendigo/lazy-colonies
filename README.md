# Lazy Colonies

A "Lazy Bastard" style, hands-off challenge for the [MineColonies](https://minecolonies.com) mod, inspired by Factorio's
*Lazy Bastard* achievement (win while handcrafting no more than 111 items).

**The idea:** you are the planner, not the labourer. You pick sites, place schematics and manage
colonists. The colonists do all the gathering, crafting and building.

> Status: draft. Rules and the supporting datapack are still being designed.
> See [`docs/design-notes.md`](docs/design-notes.md) for the working plan and open questions.

---

## The rules

### You may

- **Plan.** Choose sites and place schematics / work orders with the build tool.
- **Manage.** Hire, assign and prioritise colonists; choose research; set hut options.
- **Haul.** Move items: empty the supply camp, fill requests from the warehouse, carry items
  between huts before couriers exist. Moving items is fine; *converting* them is not.
- **Herd.** Lead in **one breeding pair** of animals per animal hut (Shepherd, Cowhand,
  Swineherd, Chicken Herder, Rabbit Hutch). Colonists can't catch animals, so this counts as hauling.
- **Defend yourself.** Fighting is allowed, but anything that drops is not colony property — see
  [Loot](#loot).

### You may not

- **Craft anything.** Not even hut blocks — those are made by colonists (see the datapack below).
- **Gather.** No mining, chopping, farming or fishing once the supply camp's stock runs out.
- **Process.** No smelting, cooking, brewing or any other item conversion.
- **Place blocks** — the builders do that. Exception: chests and torches during initial setup.

### Loot

Items from mob drops, dungeon chests, raids, etc. may **not** be fed into the colony.
*(Open question — a looser variant allows guard-tower drops, since colonists earned them.)*

### Dead ends

If no colonist can ever produce an item in your modpack:

1. **Strict:** you can't have it. Change the schematic, or skip the research.
2. **Exemption budget (optional):** you get **111 lazy exemptions** for the whole game,
   in honour of the Factorio original. Each item you obtain by hand costs one. Keep a tally.

---

## Starting conditions

The supply camp (or supply ship) is the only starting inventory. It contains:

| Item | Why |
|---|---|
| Town Hall | Needed to found the colony (vanilla behaviour) |
| Build Tool | Needed to place schematics (vanilla behaviour) |
| Builder's Hut block | The first hut, and the builder builds everything else |
| Materials for the Builder's Hut | So the first builder can actually start |
| Forester's Hut block | The source of wood |
| Sawmill hut block | The Sawmill makes every other hut block (see below) |
| A few beds | For the Builder's Hut and first Residence, before the wool chain exists |

This requires a **custom supply camp** in a style pack — see the design notes.

---

## Colony-made hut blocks

With the included datapack, **the Sawmill's Carpenter learns hut block recipes automatically**,
gated by the Sawmill's level and by research. The Sawmill's level becomes your tech tree.

| Unlock | Hut blocks |
|---|---|
| Sawmill level 1 | Warehouse, Courier, Residence, Farmer, Fisher, Miner, Shepherd, Fletcher |
| Sawmill level 2–3 | Cook, Smeltery, Stonemason, Blacksmith, Guard Tower, Dyer |
| Research-gated | University and late-game huts, tied to the research that normally unlocks them |

*Tiers are a first draft — see the design notes for balancing.*

---

## Suggested opening

1. Place the supply camp; haul the Builder's Hut materials to the build site.
2. **Builder's Hut** → **Town Hall**
3. **Forester** → **Sawmill** (hut blocks start flowing)
4. **Miner** → **Warehouse + Courier** (you can stop hauling)
5. **Farmer / Fisher → Cook** (food)
6. **Shepherd → Fletcher** (beds) — add a **Dyer** if your style uses coloured beds
7. **Stonemason, Smeltery, Blacksmith** (building materials and tools)
8. **University** (research and late-game unlocks)

---

## Victory conditions

*To be decided.* Ideas:

- Reach Town Hall level 5.
- Reach a target population (e.g. 50 colonists).
- Complete a set of research branches.

---

## Requirements

- Minecraft: *TBD*
- MineColonies: *TBD*
- The challenge datapack (not yet written) and the custom supply camp style pack (not yet written)

## Credits

Challenge inspired by Factorio's *Lazy Bastard* achievement. MineColonies is made by the
MineColonies team; this project is a fan-made challenge and is not affiliated with them.
