# New Treasure Game — Game Design (v0.1)

Planning document for a new Roblox game, built from an empty Baseplate.
All numbers are placeholders to tune in playtests.

## 0. Decisions
- **Name:** not chosen yet. Shortlist: *Cursed Gold*, *X Marks the Spot*, *Dig to Riches*, *Pirate's Plunder*, *Dead Man's Chest*. Search Roblox before picking.
- **Genre:** simulator. One big island, zones unlocked with coins and level, tool upgrades, rebirth.
- **Hook:** dark pirate look in the style of the Roblox game "Fisch", plus **cursed treasure** (worth more, but risky to carry).
- **Tone:** spooky pirate fantasy, no blood or gore.
- **Budget:** $100 for ads, spent in one burst after the first 5 minutes are proven.
- **Relation to Steal A Treasure:** separate game. Ideas, rarity colours, UI style and analytics setup may be reused.

## 1. Pitch
Dig for treasure across one big cursed island. Sell your loot at the harbour, buy better shovels and bigger backpacks, and unlock deeper, darker areas. Carry cursed treasure for huge payouts, if the ghosts don't catch you. Rebirth for permanent bonuses and do it all again, faster.

**Pillars**
1. First reward within 60 seconds.
2. Always a next goal on screen (next upgrade, next zone, next rebirth).
3. Each zone looks and feels different.
4. Risk is optional: safe players progress, brave players progress faster.

## 2. Core Loop
1. Walk to a dig spot in an unlocked zone.
2. Hold to dig: get a random treasure (rarity roll).
3. Backpack fills up.
4. Return to the hub Merchant: **Sell All** for coins.
5. Spend coins on shovel, backpack and the next zone.
6. Unlock all zones, then **rebirth** for a permanent multiplier.

A loop should take about 1–3 minutes. No long travel: zones sit next to each other and next to the hub.

## 3. Map Layout
One island. The hub sits at one end; zones follow in a line, each behind a gate.

```
[ Hub: Harbour ] → [1 Shipwreck Shore] → [2 Smugglers' Cove] → [3 Frostfang Ruins] → [4 Cursed Jungle] → [5 Sunken Temple]
```

| Hub spot | Function |
|---|---|
| Spawn | Start point, signpost to the first zone |
| Merchant | Sell All |
| Shovel shop | Shovel upgrades |
| Backpack shop | Backpack upgrades |
| Hexbreaker | Cleanse cursed treasure (see section 6) |
| Rebirth altar | Rebirth |
| Leaderboards | Top coins, top rebirths |

**Gates:** a glowing barrier between zones. A sign shows the zone name, coin cost and level needed. Unlocked gates become invisible and walkable for that player only (client-side collision), and the server also refuses digs in locked zones.

## 4. Zones

| # | Zone | Look | Cost | Level | Value mult |
|---|---|---|---|---|---|
| 1 | Shipwreck Shore | Dusk beach, wrecks, palms | free | 1 | x1 |
| 2 | Smugglers' Cove | Foggy rocks, ghost lanterns | 1,500 | 5 | x8 |
| 3 | Frostfang Ruins | Snow, ice spikes, ruins | 20,000 | 10 | x60 |
| 4 | Cursed Jungle | Dark jungle, green curse glow | 250,000 | 18 | x450 |
| 5 | Sunken Temple | Flooded stone temple, gold light | 3,000,000 | 26 | x3,500 |

Each zone has 5 treasures (one per rarity), its own lighting tint and its own decoration. New zones are added in updates by the same template.

| Zone | Common | Uncommon | Rare | Epic | Legendary |
|---|---|---|---|---|---|
| Shipwreck Shore | Rusty Coin | Silver Ring | Captain's Compass | Jeweled Dagger | Pirate King's Crown |
| Smugglers' Cove | Old Bottle | Smuggler's Flask | Ghost Lantern | Cursed Cutlass | Skull Goblet |
| Frostfang Ruins | Frozen Coin | Ice Shard | Rune Tablet | Frost Amulet | Frozen Crown |
| Cursed Jungle | Bone Charm | Jade Idol | Serpent Mask | Voodoo Totem | Heart of the Jungle |
| Sunken Temple | Ancient Coin | Golden Chalice | Sea God's Pearl | Trident Shard | Poseidon's Trident |

## 5. Digging
- Sparkling dig spots spawn in each zone (about 12 per zone, respawn after ~4 s).
- Interacting with a spot starts that zone's **minigame** (section 5a). The result sets the treasure's quality.
- Server checks: zone unlocked, player near the spot, backpack not full, dig cooldown.
- **First dig ever:** guaranteed Rare with a light pillar and a "Lucky find!" banner.

## 5a. Zone Minigames
Every zone has its own minigame that matches its theme, so each unlock also feels like a new way to play. All of them:
- take **3–8 seconds**;
- work with **one tap/click** (or one key), so they are fine on phones;
- are easy to learn and harder to master; the first zone's game is the easiest;
- never give nothing: even a bad result gives a treasure.

| # | Zone | Minigame | How it plays | Shovel helps by |
|---|---|---|---|---|
| 1 | Shipwreck Shore | **Timing Dig** | A marker swings across a bar; tap when it's in the gold zone. 3 hits. | Slower marker |
| 2 | Smugglers' Cove | **Lockpick** | Pick a smuggler's crate: 3 pins bounce up and down; tap when each pin is in the shrinking gold band. | Wider gold band |
| 3 | Frostfang Ruins | **Rune Memory** | Ice runes light up in a sequence (3–5 runes); repeat it by tapping them. | Runes shown longer |
| 4 | Cursed Jungle | **Spirit Tug** | A cursed spirit fights back (Fisch-style reel): hold to keep your bar over the moving green orb until the progress meter fills. | Bigger bar, slower orb |
| 5 | Sunken Temple | **Temple Seal** | Tap 3 stone rings to rotate them until their symbols line up before the water rises. | More time before the water |

### Results
| Result | Effect |
|---|---|
| **Perfect** (no mistakes) | +50% luck and x1.25 value, gold flash and sound |
| **Good** | normal roll |
| **Poor** (too many mistakes or time ran out) | −50% luck (mostly Common) |

- A **Perfect streak** (5 Perfects in a row) gives a bonus treasure from the same zone, to reward skill.
- Mastery: after 100 Perfects in a zone the player unlocks **Quick Dig** there, which skips the minigame and counts as Good. Good for grinding without making skill useless.
- Cursed treasure (section 6): carrying it makes every minigame a bit harder (faster marker, smaller bands), adding to the risk.

### Rules for adding new zones
Each new zone gets either a new minigame or a themed twist on an existing one (for example, Timing Dig with moving gold zones in a storm zone). Ideas for later zones: **Bellows** (keep the forge heat in a band) for a volcano zone, **Dive** (collect treasure before air runs out) for an underwater zone, **Cannon Aim** (hit floating chests) for a sea zone.

### Anti-cheat
The server picks each minigame's parameters (speeds, sequence, ring positions) and sends them when the game starts. The client sends back its inputs with timestamps. The server checks the inputs against the parameters and a minimum play time, then decides the result itself. A result the client merely claims is never trusted.

### Rarities
| Rarity | Chance | Base value | XP | Colour |
|---|---|---|---|---|
| Common | 60% | 5 | 2 | #AAAAAA |
| Uncommon | 25% | 15 | 4 | #5BBF6A |
| Rare | 10% | 50 | 8 | #4A90E2 |
| Epic | 4% | 200 | 20 | #A855F7 |
| Legendary | 1% | 1,000 | 60 | #F59E0B |

Treasure value = base value × zone multiplier. XP = rarity XP × zone number.
Luck multiplies the weights of Rare and better.

## 6. Cursed Treasure (the hook)
- About 8% of Rare-or-better finds are **cursed**: green flicker, worth **x2**.
- While carrying cursed treasure: walk speed −25% and ghosts in that zone hunt you.
- Hit by a ghost too often = knocked out, **all carried treasure drops** on the spot (reserved for you for 20 s, then anyone can grab it, gone after 60 s).
- The Merchant refuses cursed treasure. Cleanse it at the Hexbreaker for 10% of its value, then sell it for the full x2.
- Players who never pick up cursed treasure are never hunted, so it stays optional.

## 7. Progression

### Shovels
"Ease" makes every minigame easier (see the "Shovel helps by" column in section 5a).

| # | Shovel | Cost | Luck | Ease |
|---|---|---|---|---|
| 1 | Rusty Shovel | free | +0% | 0% |
| 2 | Iron Shovel | 150 | +10% | 5% |
| 3 | Steel Shovel | 1,200 | +25% | 10% |
| 4 | Silver Shovel | 9,000 | +45% | 15% |
| 5 | Gold Shovel | 70,000 | +70% | 20% |
| 6 | Ghost Shovel | 600,000 | +100% | 25% |
| 7 | Cursed Gold Shovel | 5,000,000 | +150% | 30% |

### Backpacks
| # | Backpack | Cost | Slots |
|---|---|---|---|
| 1 | Pouch | free | 10 |
| 2 | Satchel | 100 | 20 |
| 3 | Sack | 800 | 35 |
| 4 | Chest Pack | 6,000 | 60 |
| 5 | Sea Trunk | 50,000 | 100 |
| 6 | Vault Pack | 400,000 | 160 |
| 7 | Kraken Hold | 3,000,000 | 250 |

### Levels
XP needed for the next level = `floor(40 × level^1.5)`. Levels gate zones together with coins, so players can't skip ahead by saving coins alone.

### Rebirth
- Needs all zones unlocked. Cost = `5,000,000 × 3^rebirths`.
- Resets coins, zones, shovel, backpack and carried treasure. Keeps level, collection and cosmetics.
- Each rebirth: **+50% sell value** and **+10% luck**, permanent, plus a title/cosmetic.

**Pacing targets:** first zone unlock in ~5 min, zone 3 in ~30 min, first rebirth in ~3–4 hours.

## 8. Retention
- Daily reward streak (7 days, day 7 a big chest).
- Collection book: find all 5 treasures of a zone for a permanent +5% in that zone.
- Weekend event "Golden Tide": +25% luck on Saturday and Sunday.
- Badges: first treasure, each zone, first rebirth, 7-day streak.
- One new zone or feature per update, with the update named in the game title, e.g. `[NEW ZONE] Cursed Gold`.

## 9. Monetization (fair, no pay-to-win walls)
| Game pass | Effect |
|---|---|
| x2 Coins | Double sell value |
| VIP | +10% luck, +25% backpack, VIP tag |
| Auto-Sell | Sell from anywhere |
| Lucky Charm | +25% luck |

Developer products: coin packs, a 15-minute x2 luck potion, skip-a-zone-gate (later).

## 10. Social
- Server size 20–25.
- Optional later: crews (+2% sell per crewmate nearby), PvP stealing zones where dropped loot can be taken.

## 11. Art Direction
- Dark, atmospheric look inspired by "Fisch": Future lighting, Atmosphere, fog, bloom, sun rays, colour grading; dusk or night skies.
- Each zone has its own colour identity:

| Zone | Palette |
|---|---|
| Hub | Lantern amber #E0A04A, dark wood #3B2A20, sea teal #1F4A52 |
| Shipwreck Shore | Warm sand, wet wood, dusk orange |
| Smugglers' Cove | Murky teal #2F5F5B, ghost cyan #7FE0D0 |
| Frostfang Ruins | Ice blue #8FB8D6, deep indigo #1E2A44 |
| Cursed Jungle | Dark green, curse green #7CFC5B |
| Sunken Temple | Slate blue, molten gold #FFB347 |

- Treasure always glows in its rarity colour so it is the brightest thing on screen.
- UI: dark wood and worn parchment, brass trim, readable on phones.
- **Must run well on low-end phones:** keep part counts moderate, few lights, turn off shadows on small decor.

## 12. Technical Plan (Roblox)
- Server-authoritative: coins, inventory, treasure rolls, unlocks, rebirth.
- DataStore save with retries, autosave and BindToClose.
- Modules: `Config` (all numbers above), `PlayerData`, `Dig`, `Shop`, `Zones`, `Rebirth`, `Curse`/`Ghosts`, `Retention`, `Analytics`.
- Minigames: one server validator and one client UI module per minigame (`TimingDig`, `Lockpick`, `RuneMemory`, `SpiritTug`, `TempleSeal`), all behind a shared interface (`start(params)` → inputs → `judge(params, inputs)` → Perfect/Good/Poor). Each zone's config names its minigame and difficulty, so a new zone can reuse one.
- Player stats replicated to the client as attributes; one RemoteFunction for shop actions, one RemoteEvent for notifications.
- Analytics funnel: joined → first dig → first sale → first upgrade → zone 2 unlocked → first rebirth.

## 13. Build Roadmap
1. **Playable core:** island, hub, zones 1–3, digging with the Timing Dig, Lockpick and Rune Memory minigames, selling, shovel/backpack upgrades, zone gates, levels, saving, basic HUD.
2. **Playtest with friends:** do they keep playing past 10 minutes? Which minigame do they like most and least?
3. **The hook:** cursed treasure, ghosts, Hexbreaker, knockout drop.
4. **Zones 4–5** with Spirit Tug and Temple Seal, Perfect streaks, Quick Dig, and rebirth.
5. **Polish:** lighting, sounds, UI art, first-dig lucky find, mobile check.
6. **Launch kit:** game passes, icon, thumbnails, badges, daily rewards.
7. **Launch:** spend the $100 in one burst over a weekend, watch retention in the Creator Dashboard.

## 14. Open Questions
- Final name.
- Cursed treasure chance and ghost difficulty.
- PvP stealing zones: in v1 or later?
- Level-gate numbers after the first playtest.
- Minigame difficulty curves and the exact Perfect/Good/Poor thresholds.
