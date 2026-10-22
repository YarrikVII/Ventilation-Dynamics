# Ventilation Dynamics changelog

## 0.26.0

### Integrated Mod Manager
- Every server setting can be changed in game through Integrated Mod Manager, with labels, descriptions and sensible ranges. Changes apply right away (Asphyxia integration needs a server restart; players see new haze and fog thresholds after rejoining).
- Editing the settings file by hand still works the same; it's read at server start.

### Server settings file
- New `ModConfig/ventilationdynamics.json`, created with defaults on first start: thresholds, gas sources, firedamp, drafts, ventilation blocks, breathing gear, duct crawling, Asphyxia integration and performance budgets. See SETTINGS.md for every setting.
- The thresholds that affect what players see (suffocation and the firedamp range) are sent to clients, so haze and screen fog stay in step with the server's settings.
- `/vent sources` and `/vent blast` still work per world; the settings file gives the defaults for worlds that haven't changed them.

### Death messages
- Deaths from chokedamp, rich firedamp, and firedamp flashes and explosions get their own messages instead of the generic ones. Can be turned off with `DeathMessages`.

## 0.25.0

### Asphyxia: Rebreathed
- The air bladder and the air-hose hood now work with Asphyxia: when its air is unbreathable, the hood (near a fresh-air vent) or the bladder supplies your air, and masks stretch your breath, the same as without it.
- Light warnings now line up with Asphyxia's suffocation point: torches dim partway to danger and go out just before the air turns deadly, as they do without Asphyxia. Before, they went out in air that was only mildly bad.

## 0.24.2

### Performance
- Draft calculations run about 4–5 times faster (about 80 ms down to about 17 ms per pass in a large test cave with 6 openings, ducts and heat sources), so the server no longer hitches every few seconds in big mines.
- Torches, oil lamps, lanterns and firedamp ignition now share one scan per second, limited to gas within 64 blocks of a player, instead of four separate scans of every gas chunk loaded.

## 0.24.1

### Ventilation furnace faces you
- The furnace is now placed with its front (the stoking arch) facing the player, in any of four directions.
- Furnaces already placed keep working where they are; breaking one gives the new, turnable furnace.

## 0.24.0

### In-game guide
- Seven handbook guide pages: Overview (with a quick start), Bad Air, Reading the Air, Moving Air, Ducts and Fittings, Breathing Gear, and With Asphyxia.

## 0.23.4

### Renamed: Gas Dynamics is now Ventilation Dynamics
- New mod ID `ventilationdynamics`. **Not compatible with worlds from earlier test builds:** blocks, items and saved gas from Gas Dynamics won't carry over.
- Commands are now `/vent ...` (server) and `.ventclient` (client).

### Compatibility with Asphyxia: Rebreathed
When Asphyxia: Rebreathed is installed, Ventilation Dynamics becomes its ventilation layer:
- Its own gases (chokedamp, firedamp), gas sources, breathing changes and firedamp ignition switch off. Asphyxia handles all gases.
- Drafts from openings, climate, furnaces and other heat, ducts, scoops, extractors and vents clear Asphyxia's gases along their routes, more for stronger drafts.
- Torches, oil lamps and lanterns dim (and torches go out) by Asphyxia's breathable-air level, on the same thresholds.
- Both breathing masks filter smoke, coal dust and silica dust under Asphyxia's gas mask rules.
- Works without a hard dependency: it talks to Asphyxia only through its public gas helper and its own gas exchange queue.

### Breathing masks
- **Simple breathing mask** (renamed from the cloth mask): 2 flax fibers or 3 dry grass. Same effect as before.
- **Linen breathing mask** (new): cloth, flax twine, cloth. Same effect, three times the durability.

## 0.22.0 (first playable version)

Mine air you have to manage. Built for Vintage Story 1.22. Install on both server and clients.

### Gases
- **Chokedamp** is heavier than air. It sinks, pools in low points, spills off ledges, and is unbreathable at density 64 or more. It drains the normal oxygen bar, then deals suffocation damage.
- **Firedamp** is lighter than air. It rises, gathers at ceilings, and vents out of openings. Between density 40 and 149 it ignites at any open flame; 150 and above is too rich to burn but unbreathable.
- **Ignition** burns every connected cell in the explosive range: a small amount gives a flash fire that burns and sets alight anyone inside, a large amount also explodes. Rich pockets burn at their edges, and torches beside them act as pilot lights.
- **Blast modes** (per world, `/vent blast`): `full` (default, breaks blocks across half the blast radius), `crater`, `creatures`, `flash`.

### Where gas comes from
- **Coal:** breaking lignite, bituminous coal or anthracite releases a little firedamp.
- **Stagnant air:** enclosed space below the depth line (16 blocks under sea level) slowly gathers chokedamp near players.
- **Stale air on arrival:** deep chunks are seeded with chokedamp the first time a player comes near, deeper meaning more. Each chunk once.

### Air movement
- Gas diffuses, follows gravity, and vents to the sky with the wind. Pits and shafts hold gas; open ground and forest floors vent.
- **Drafts:** two openings at different heights move air along the routes between them, carrying gas toward the exhaust. Long routes are weaker.
- **Climate:** natural drafts follow the temperature difference between the mine (10°C) and outside. Hot weather reverses them.
- **Heat:** the ventilation furnace and lit firepits, forges, bloomeries, clay ovens and charcoal pits add draw; heat rises like smoke to find its opening and pulls air from nearby side passages.

### Ventilation blocks
- **Ventilation furnace:** cobblestone plus fired bricks or fired bowls. Fuel it like a firepit; it burns for up to an hour of real time.
- **Wooden air duct:** a sealed passage between its open ends. Shapes itself into straights, corners and 3- to 6-way junctions.
- **Wind scoop:** on a duct's surface end, pushes fresh air in with the wind.
- **Wind extractor:** on a duct's surface end, pulls stale air out with the wind.
- **Air vent:** at the tip of a duct or branch, spreads fresh air out (or draws stale air in) around it. Faces its duct.
- **Duct crawling:** sneak and right-click any part of a duct run to crawl to its far end. Takes time, in the dark.

### Light in bad air
| Light | In chokedamp | In firedamp |
|---|---|---|
| Torch | dims from 24, goes out at 56 | ignites it |
| Oil lamp | dims from 24, never goes out | ignites it |
| Lantern | dims by 5 light levels, never goes out | safe |

### Breathing gear
- **Cloth breathing mask** (face): your air lasts 1.5 times longer; wears with use.
- **Air bladder** (hotbar or backpack): about 20 seconds of air, used first, refills in clean air.
- **Air-hose hood** (head): breathe normally within 8 blocks of a vent blowing fresh air in.

### Seeing it
- Chokedamp shows as a low grey haze, firedamp as rising yellow-green mist, shimmer and motes, both only at dangerous levels and only where there's light. Screen fog and tint while you're breathing it.
- Dust drifts along drafts; air puffs at working vents, scoops and extractors.

### Commands (server operators)
`/vent read`, `/vent show`, `/vent draft`, `/vent stats`, `/vent add`, `/vent clear`, `/vent blast`, `/vent sources`, `/vent messages` (ignition details in your chat, off by default). Client: `.ventclient`.

### Changes since the last test build
- Ignition details no longer appear in everyone's chat; they go to the server log, and to chat only with `/vent messages on`.
- Mod description written.

### Known limits
- Simulation is per 2×2×2 block cell, so gas edges look blocky in the debug overlay.
- Touching duct runs join, so leave a gap between separate runs.
- Lanterns and torches use patches on vanilla files; another mod replacing the lantern's class would clash.
- Balance numbers (seeding, stagnation, draft strength, gear) are first passes and need playtesting.
