# TDS AutoStrat - Complete Documentation

Welcome! This guide will help you understand and use the TDS AutoStrat script. Don't worry if you're new to scripting - we'll explain everything in simple terms.

---

## Table of Contents

1. [Configuration](#configuration)
2. [Webhook Settings (Optional)](#webhook-settings-optional)
3. [Loading the TDS Library](#loading-the-tds-library)
4. [Setting Up Your Game (Loadout, Mode, Map)](#setting-up-your-game)
5. [Building Your Strategy](#building-your-strategy)
6. [API Reference](#api-reference)
7. [Advanced Features](#advanced-features)
8. [Troubleshooting & Tips](#troubleshooting--tips)

---

## Configuration

The configuration section lets you control how the script behaves. Think of it like the **settings menu** of your script!

### Basic Settings

```lua
_G.AutoStrat = true
_G.AutoSkip = false
_G.AutoPickups = true
```

### What Each Setting Does

**What is `_G`?**  
`_G` means "global variable" - it's a variable that can be used anywhere in the script. Think of it as a master control panel!

| Setting | Type | What It Does |
|---------|------|-------------|
| `_G.AutoStrat` | true/false | Runs the strategy automatically without you having to do anything |
| `_G.AutoSkip` | true/false | Automatically votes to skip waves (if your game library supports it) |
| `_G.AutoPickups` | true/false | Automatically collects dropped items and cash that fall on the ground |

### Understanding Data Types

The script uses **data types** - these are different kinds of information:

- **(bool)** = Boolean - Only two choices: `true` or `false` (like an on/off switch)
- **(string)** = Text - Words, sentences, links (written in quotes like `"hello"`)
- **(number)** = Numbers - Coordinates, amounts, values

---

## Webhook Settings (Optional)

Webhooks let the script send you updates to Discord when matches finish!

```lua
_G.SendWebhook = false 
_G.Webhook = "WEBHOOK_URL_HERE"
```

### How to Set Up Discord Webhooks

1. **`_G.SendWebhook`** - Turn notifications on/off
   - `true` = You'll get Discord messages when matches end
   - `false` = No Discord messages

2. **`_G.Webhook`** - Your Discord webhook link
   - Replace `"WEBHOOK_URL_HERE"` with your actual Discord webhook URL
   - The URL should look something like: `https://discord.com/api/webhooks/123456789/abcdefg...`

**How to Get Your Webhook URL:**
- Go to your Discord server
- Right-click a channel → Edit Channel → Integrations → Webhooks
- Create a new webhook and copy the URL
- Paste it into the `_G.Webhook` setting

---

## Loading the TDS Library

This section loads the "engine" - the main code that makes everything work. **You can skip reading this part** - just know that it's downloading and running the library code.

```lua
local TDS = loadstring(game:HttpGet("https://raw.githubusercontent.com/DuxiiT/auto-strat/refs/heads/main/Library.lua"))()
TDS:Addons()
```

### What's Happening Here?

- `game:HttpGet()` = Downloads code from the internet (like downloading a file)
- `loadstring()` = Runs that code and returns the TDS object
- `TDS:Addons()` = Loads extra features and plugins for the library

---

## Setting Up Your Game

Before your strategy runs, you need to tell the script which towers to use, what difficulty level, and which map to play.

### 1. Choosing Your Towers (Loadout)

```lua
TDS:Loadout("Mercenary Base", "Military Base", "Ranger", "Accelerator", "Engineer")
```

**Important Notes:**

- **You must own these towers in the game** and have them equipped
- **You can only bring 5 towers maximum** - list only the first 5 towers in the script
- The order matters - towers will be equipped in the order you list them

### 2. Selecting Game Difficulty (Mode)

```lua
TDS:Mode("Hardcore")
```

**Available Difficulty Modes:**

```
"Easy"
"Casual"
"Intermediate"
"Molten"
"Fallen"
"Challenge"
"Hardcore"
"Frost"
```

### 3. Choosing Your Map and Modifiers (GameInfo)

```lua
TDS:GameInfo("Wrecked Battlefield", {})
```

**Understanding This Function:**

- First part: `"Wrecked Battlefield"` = The map name
- Second part: `{}` = Game modifiers (the curly brackets mean it's empty right now)

### 4. Adding Game Modifiers

If you want to add modifiers (these change the game difficulty), fill in that `{}` part:

```lua
TDS:GameInfo("Simplicity", {HiddenEnemies = true, ExplodingEnemies = true})
```

**All Available Modifiers:**

```
HiddenEnemies, Glass, ExplodingEnemies, Limitation, Committed, 
HealthyEnemies, SpeedyEnemies, Quarantine, Fog, FlyingEnemies, 
Broke, Jailed, Inflation
```

Each modifier changes how the enemies behave - stronger, faster, hidden, etc.

---

## Building Your Strategy

Now for the fun part - building your game strategy! Think of this like a recipe with steps.

### Part A: Placing Towers

Placing towers is how you start your strategy. Each tower is placed at specific coordinates.

```lua
TDS:Place("Military Base", 0.7064, 1.8626, 42.2678, 1,0,0,0,1,0,0,0,1) -- 1
```

### Understanding the Place Function

```lua
TDS:Place(TowerName, X, Y, Z)
-- OR with optional rotation:
TDS:Place(TowerName, X, Y, Z, RotationMatrix)
```

| Parameter | What It Is | Required? | Example |
|-----------|-----------|-----------|---------|
| Tower Name | The name of your tower | Yes | `"Military Base"` |
| X, Y, Z | Coordinates where it goes | **Yes** | `0.7064, 1.8626, 42.2678` |
| Rotation Matrix | How the tower is rotated | No (Optional) | `1,0,0,0,1,0,0,0,1` |

**About the Rotation Matrix:**

⚠️ **Most users don't need this!** The rotation matrix controls tower rotation, but **it's completely optional**. In most cases, you only need to specify the X, Y, Z coordinates:

```lua
-- Simple way (recommended for beginners)
TDS:Place("Military Base", 0.7064, 1.8626, 42.2678)

-- With rotation (only if you need specific rotation)
TDS:Place("Military Base", 0.7064, 1.8626, 42.2678, 1,0,0,0,1,0,0,0,1)
```

The 9 numbers at the end are a math concept called a "rotation matrix." Unless you're doing something advanced, just skip it and use only X, Y, Z!

### Tracking Tower Placements

Notice the `-- 1` at the end? This is a **comment** - it helps you keep track of the order!

**Simple way (just X, Y, Z):**
```lua
TDS:Place("Tower 1", 0.5, 1.8, 42.0) -- 1
TDS:Place("Tower 2", 2.3, 1.8, 35.5) -- 2
TDS:Place("Tower 3", 4.1, 1.8, 28.0) -- 3
TDS:Place("Tower 4", 5.9, 1.8, 20.5) -- 4
TDS:Place("Tower 5", 7.7, 1.8, 13.0) -- 5
```

**OR with rotation matrix (if you need it):**
```lua
TDS:Place("Tower 1", 0.5, 1.8, 42.0, 1,0,0,0,1,0,0,0,1) -- 1
TDS:Place("Tower 2", 2.3, 1.8, 35.5, 1,0,0,0,1,0,0,0,1) -- 2
TDS:Place("Tower 3", 4.1, 1.8, 28.0, 1,0,0,0,1,0,0,0,1) -- 3
TDS:Place("Tower 4", 5.9, 1.8, 20.5, 1,0,0,0,1,0,0,0,1) -- 4
TDS:Place("Tower 5", 7.7, 1.8, 13.0, 1,0,0,0,1,0,0,0,1) -- 5
```

**Why number them?** Because future functions will refer to towers by their placement number (like "upgrade tower #2" or "sell tower #3").

### Part B: Upgrading Towers

After placing towers, you'll want to upgrade them to make them stronger!

**Basic Upgrade (Path 1 - Default):**
```lua
TDS:Upgrade(5)
```
This upgrades tower #5 by one level along path 1 (the default path).

**Upgrade with Specific Path:**
```lua
TDS:Upgrade(6, 2)
```
This upgrades tower #6 along **path 2** instead of path 1.

**Real-World Example - Hacker Tower:**

Some towers have special upgrade paths at the end. The **Hacker** tower is a great example:

```lua
TDS:Place("Hacker", 0.5, 1.8, 42.0) -- 1

-- Upgrade it multiple times (path 1 - damage focused)
TDS:Upgrade(1)  -- Upgrade 1
TDS:Upgrade(1)  -- Upgrade 2
TDS:Upgrade(1)  -- Upgrade 3
TDS:Upgrade(1)  -- Upgrade 4
TDS:Upgrade(1, 1)  -- Upgrade 5, Path 1 = Damage Variant ⚡

-- OR for support variant instead:
TDS:Upgrade(1, 2)  -- Upgrade 5, Path 2 = Support Variant 🛡️
```

**How Upgrade Paths Work:**

- Most upgrades progress normally
- **At certain "milestone" upgrades**, the tower splits into two paths
- **Path 1** = Usually the damage/offensive variant
- **Path 2** = Usually the support/utility variant
- You choose which path by specifying the path number in the `TDS:Upgrade()` function

---

## API Reference

This section lists all the functions you can use to control the script. Think of these as "commands" you can give to the script.

### Core Engine Methods

These are the main functions that do things in the game.

#### Placing & Selling

```lua
TDS:Place(name, x, y, z)     -- Places a tower at the given position
TDS:Sell(index)              -- Sells tower #index
TDS:SellAll()                -- Sells all towers
```

#### Upgrading

```lua
TDS:Upgrade(index, path)     -- Upgrades tower #index (path 1 or 2)
```

#### Targeting

```lua
TDS:SetTarget(index, mode)   -- Sets how tower targets enemies
```

**Targeting Modes:**
```
"First"   - Targets the first enemy on the path
"Strong"  - Targets the strongest enemy
"Last"    - Targets the last enemy on the path
"Random"  - Targets randomly
```

#### Game Speed

```lua
TDS:TimeScale(value)         -- Changes game speed
```

**Valid Speeds:**
```
0.5  -- Half speed
1    -- Normal speed (default)
1.5  -- 1.5x faster
2    -- Double speed
```

#### Waves & Waves

```lua
TDS:VoteSkip(wave)           -- Votes to skip to a specific wave
TDS:GetWave()                -- Returns the current wave number
TDS:RestartGame()            -- Triggers the game restart
```

#### Lobby Functions

```lua
TDS:Loadout(...)             -- Sets which towers you bring
TDS:Mode(difficulty)         -- Sets game difficulty
TDS:GameInfo(map, modifiers) -- Sets map and modifiers
TDS:StartGame()              -- Starts the game from lobby
```

---

### SetOption Function

Use this for towers that have special options or settings.

```lua
TDS:SetOption(index, "OptionName", "Value", requiredWave)
```

**Tower Options:**

**1. Mercenary Base**
```lua
TDS:SetOption(1, "Unit 1", "Grenadier")  -- or 2, 3 for different units
-- Options: "Grenadier", "Rifleman", "Riot Guard", "Field Medic"
```

**2. Trapper**
```lua
TDS:SetOption(2, "Trap", "Spike")        -- or "Landmine"
```

**3. DJ Booth**
```lua
TDS:SetOption(3, "Track", "Green")       -- or "Red", "Purple"
```

---

### Ability Function

Special towers have special abilities. Use this to trigger them!

```lua
TDS:Ability(index, "AbilityName", data, loop)
```

**What each parameter means:**

- `index` = Which tower (1, 2, 3, etc.)
- `"AbilityName"` = The name of the ability
- `data` = Extra information the ability needs (optional)
- `loop` = `true` to repeat the ability automatically, `false` to use it once

#### Tower Abilities

**Commander**
```lua
TDS:Ability(1, "Call Of Arms")          -- Boosts nearby towers
TDS:Ability(1, "Support Caravan")       -- Sends support
```

**DJ Booth**
```lua
TDS:Ability(2, "Drop The Beat")         -- Blasts enemies
```

**Medic**
```lua
TDS:Ability(3, "Ubercharge")            -- Heals & buffs
```

**Hacker**
```lua
TDS:Ability(4, "Hologram Tower", {
    towerToClone = 1,                   -- Clone tower #1
    towerPosition = Vector3.new(0, 5, 10)  -- Place it here
})
```

**Pursuit**
```lua
TDS:Ability(5, "Patrol", {
    position = Vector3.new(0, 5, 10)    -- Send it here
})
```

**Gatling Gun**
```lua
TDS:Ability(6, "FPS", {
    enabled = true                      -- Turn it on/off
})
```

**Brawler**
```lua
TDS:Ability(7, "Reposition", {
    position = Vector3.new(0, 5, 10)    -- Move tower to here
})
```

**Mercenary Base / Military Base**
```lua
TDS:Ability(1, "Air-Drop", {
    pathName = 1,
    dist = 100,                         -- Where on the path (0-200)
    pointToEnd = 0,
    directionCFrame = CFrame.new()
})

TDS:Ability(2, "Airstrike", {
    pathName = 1,
    dist = 150,                         -- Where on the path (0-200)
    pointToEnd = 0,
    directionCFrame = CFrame.new()
})
```

---

### AutoChain Function

This makes towers use their abilities in a chain automatically!

```lua
TDS:AutoChain(1, 2, 3)     -- Towers 1, 2, and 3 use their abilities in order
```

---

## Advanced Features

### Understanding Position Values (dist / pointToEnd)

When using abilities like Air-Drop or Airstrike, you can control **WHERE on the path** the ability triggers.

```lua
dist = 0      -- Ability triggers at START of path (where enemies spawn)
dist = 156    -- Ability triggers near END of path (Simplicity map max)
dist = 999    -- Ability targets very end or stays "stuck"
```

**Different maps have different maximum distances.** Simplicity's max is about 156.

### Finding Position Values for Your Map

If you want precise control over ability positions:

1. Open a RemoteSpy (SimpleSpy, Hydroxide, etc.)
2. Use the ability manually in-game once
3. Look for the RemoteFunction call named `"Troops"` with action `"Abilities"`
4. Check the `Data` table to see your cursor's exact position value
5. Use that number in your script!

### Anti-Lag Feature

The script can reduce lag by destroying unnecessary visual elements:

```lua
_G.AntiLag = true     -- Stops you from lagging too hard
```

This removes animations, projectiles, and enemies visually to improve performance.

### 24/7 AFK Mode

For completely hands-off play, add the script to your executor's **Autoexec** folder. It will:
- Run automatically when you open the game
- Play matches 24/7
- Automatically rejoin if disconnected
- Stay active even if you go AFK

---

## Troubleshooting & Tips

### Towers Not Placing

**Problem:** Towers don't appear on the map

**Solution:** You might not have enough money! The script will retry automatically, but it can't spend money you don't have. Make sure you have enough cash before the script tries to place towers.

### Tower Indexing Not Working Right

**Problem:** After selling a tower, the numbers seem off

**Solution:** This is normal! Tower indices don't shift down when you sell:
```lua
TDS:Place("Tower A", ...) -- 1
TDS:Place("Tower B", ...) -- 2
TDS:Sell(1)               -- Tower A is gone
-- Tower B is STILL #2, not #1
```

### Webhook Not Sending Messages

**Problem:** Discord isn't getting match result notifications

**Solution:** Check these things:
1. Is `_G.SendWebhook` set to `true`?
2. Is `_G.Webhook` set to your actual Discord webhook URL?
3. Is the URL the "Raw" webhook link (not a regular channel invite)?
4. If script freezes at match end, your webhook URL might be invalid

---

## Quick Start Example

Here's a simple complete example to get you started:

```lua
-- Configuration
_G.AutoStrat = true
_G.AutoSkip = false
_G.AutoPickups = true

-- Load library
local TDS = loadstring(game:HttpGet("https://raw.githubusercontent.com/DuxiiT/auto-strat/refs/heads/main/Library.lua"))()
TDS:Addons()

-- Setup your game
TDS:Loadout("Mercenary Base", "Military Base", "Ranger", "Accelerator", "Engineer")
TDS:Mode("Hardcore")
TDS:GameInfo("Simplicity", {})

-- Build your strategy (example)
TDS:Place("Mercenary Base", 0.7064, 1.8626, 42.2678, 1,0,0,0,1,0,0,0,1) -- 1
TDS:Place("Military Base", 5.2, 1.8626, 35.5, 1,0,0,0,1,0,0,0,1) -- 2

-- Upgrade towers
TDS:Upgrade(1)
TDS:Upgrade(2)

-- Set special targeting
TDS:SetTarget(1, "Strong")

-- Use abilities
TDS:Ability(1, "Air-Drop")
```

---

## Global Configuration Summary

Here's all the global settings in one place:

| Setting | Type | Default | Purpose |
|---------|------|---------|---------|
| `_G.AutoStrat` | bool | `true` | Enable automatic strategy execution |
| `_G.AutoSkip` | bool | `true` | Auto-skip waves |
| `_G.AutoPickups` | bool | `true` | Auto-collect dropped items |
| `_G.SendWebhook` | bool | `true` | Send match results to Discord |
| `_G.Webhook` | string | `""` | Your Discord webhook URL |
| `_G.AntiLag` | bool | `true` | Reduce lag |

---

## Copyright

**TDS AutoStrat** - Copyright 2025 DUXII  
Personal use only. Do not redistribute without permission.

---

**Questions?** Make sure to check the examples above. If you're still confused about a concept, it's probably explained earlier in this guide!
