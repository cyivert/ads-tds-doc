# TDS AutoStrat - Complete Documentation

Welcome! This guide will help you understand and use the TDS AutoStrat script. Don't worry if you're new to scripting - we'll explain everything in simple terms.

---

## Table of Contents

1. [Getting Started](#getting-started)
2. [Installation & Configuration](#installation--configuration)
3. [Understanding Data Types](#understanding-data-types)
4. [Loading the TDS Library](#loading-the-tds-library)
5. [Setting Up Your Game](#setting-up-your-game)
6. [New Feature: Multiplayer & Multi-Mode](#new-feature-multiplayer--multi-mode)
7. [Building Your Strategy](#building-your-strategy)
8. [Tower Management](#tower-management)
9. [Special Abilities & Advanced Controls](#special-abilities--advanced-controls)
10. [Game Speed & Performance](#game-speed--performance)
11. [Discord Webhooks (Optional)](#discord-webhooks-optional)
12. [Advanced Features](#advanced-features)
13. [Troubleshooting & Tips](#troubleshooting--tips)

---

## Getting Started

**What is TDS AutoStrat?**

TDS AutoStrat is a script that automates your Tower Defense Strategy gameplay. It can:
- Automatically place towers at specific locations
- Upgrade and manage towers during the game
- Activate special abilities
- Skip waves automatically
- Log your match results to Discord
- Play 24/7 without you having to do anything

Think of it like a **"recipe"** for winning - you write out the steps, and the script executes them perfectly every time!

---

## Installation & Configuration

Before the script runs, you need to set up basic configuration. Think of it like the **settings menu** of your script!

### Basic Settings

```lua
_G.AutoStrat = true
_G.AutoSkip = false
_G.AutoPickups = true
_G.AntiLag = true
```

### What Each Setting Does

**What is `_G`?**  
`_G` means "global variable" - it's a variable that can be used anywhere in the script. Think of it as a master control panel!

| Setting | Type | What It Does |
|---------|------|-------------|
| `_G.AutoStrat` | true/false | Runs the strategy automatically without you having to do anything |
| `_G.AutoSkip` | true/false | Automatically votes to skip waves (if your game library supports it) |
| `_G.AutoPickups` | true/false | Automatically collects dropped items and cash that fall on the ground |
| `_G.AntiLag` | true/false | Reduces lag by removing unnecessary visual effects |

---

## Understanding Data Types

The script uses **data types** - these are different kinds of information:

- **(bool)** = Boolean - Only two choices: `true` or `false` (like an on/off switch)
- **(string)** = Text - Words, sentences, links (written in quotes like `"hello"`)
- **(number)** = Numbers - Coordinates, amounts, values

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

## New Feature: Multiplayer & Multi-Mode

**NEW!** You can now create strategies for multiplayer sessions that are completely automated!

### Understanding Multiplayer Setup

Multiplayer games require:
1. **A host** - The player who selects the map and starts the game
2. **Clients** - Players who join the host's game
3. **A party code** - A password so everyone can find each other

### Step 1: Set the Multiplayer Mode

Use `TDS:MultiMode(difficulty, totalplayers)` to set up a multiplayer game:

```lua
TDS:MultiMode("Easy", 3)  -- Easy difficulty for 3 players
TDS:MultiMode("Hardcore", 4)  -- Hardcore for 4 players
```

### Step 2: Join as Host or Client

Use `TDS:Multiplayer(role, partycode)` to join the lobby:

```lua
TDS:Multiplayer("host", "MYCODE")    -- You're the host
TDS:Multiplayer("client", "MYCODE")  -- You're a client
```

**What Each Role Does:**

- **"host"**: You select the map and settings. Must have VIP. Add `TDS:GameInfo("MAPNAME", {})` after this.
- **"client"**: You join the host's game. Add `TDS:StartGame()` after this.

**Important:** Everyone in your team must use the **exact same party code**!

### Multiplayer Example - 3-Player Team

Here's a complete example with 3 players:

**Player 1 (Host - VIP):**
```lua
TDS:MultiMode("Easy", 3)
TDS:Multiplayer("host", "AUTOSTRAT")
TDS:GameInfo("U-Turn", {})
TDS:Loadout("Tower1", "Tower2", "Tower3", "Tower4", "Tower5")
-- ... rest of your strategy
```

**Player 2 (Client):**
```lua
TDS:MultiMode("Easy", 3)
TDS:Multiplayer("client", "AUTOSTRAT")
TDS:StartGame()
TDS:Loadout("Tower1", "Tower2", "Tower3", "Tower4", "Tower5")
-- ... rest of your strategy
```

**Player 3 (Client):**
```lua
TDS:MultiMode("Easy", 3)
TDS:Multiplayer("client", "AUTOSTRAT")
TDS:StartGame()
TDS:Loadout("Tower1", "Tower2", "Tower3", "Tower4", "Tower5")
-- ... rest of your strategy
```

### Key Points for Multiplayer

- The **host** selects the map and difficulty
- **All clients** must use the same party code to join
- Everyone can have different tower loadouts and strategies
- The game starts when the host is ready and all clients vote "ready"

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

## Tower Management

Once towers are placed, you can manage them during gameplay!

### Selling Towers

Remove towers from the map to get your money back:

```lua
TDS:Sell(1)       -- Sells tower #1
TDS:SellAll()     -- Sells ALL towers at once
```

### Setting Tower Targeting

Control how towers aim at enemies:

```lua
TDS:SetTarget(1, "Strong")  -- Tower #1 targets the strongest enemy
```

**Targeting Modes:**
```
"First"   - Targets the first enemy on the path
"Strong"  - Targets the strongest enemy
"Last"    - Targets the last enemy on the path
"Random"  - Targets randomly
```

### Setting Tower Options

Some towers have special options or settings:

```lua
TDS:SetOption(index, "OptionName", "Value", requiredWave)
```

**Tower Options:**

**1. Mercenary Base / Military Base**
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

## Special Abilities & Advanced Controls

### Activating Special Abilities

Special towers have unique abilities. Use this to trigger them!

```lua
TDS:Ability(index, "AbilityName", data, loop)
```

**What each parameter means:**

- `index` = Which tower (1, 2, 3, etc.)
- `"AbilityName"` = The name of the ability
- `data` = Extra information the ability needs (optional)
- `loop` = `true` to repeat the ability automatically, `false` to use it once

### Tower Ability Reference

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

### AutoChain - Automated Ability Chains

Make towers use their abilities in sequence automatically!

```lua
TDS:AutoChain(1, 2, 3)     -- Towers 1, 2, and 3 use their abilities in order
```

This is perfect for creating chains where one tower's ability triggers before another!

### Wave Management

```lua
TDS:GetWave()              -- Returns the current wave number
TDS:VoteSkip(10)           -- Votes to skip, waits until wave 10 to skip
TDS:RestartGame()          -- Triggers game restart after match ends
```

---

## Game Speed & Performance

### Controlling Game Speed

Change how fast the game runs:

```lua
TDS:TimeScale(2)           -- 2x speed (double speed)
TDS:TimeScale(1)           -- 1x speed (normal)
TDS:TimeScale(0.5)         -- 0.5x speed (half speed)
```

**Valid Speeds:**
```
0.5  -- Half speed
1    -- Normal speed (default)
1.5  -- 1.5x faster
2    -- Double speed
```

### Unlocking Speed Tickets

If you have timescale tickets, unlock speed control:

```lua
TDS:UnlockTimeScale()      -- Unlocks speed control if you have tickets
```

### Anti-Lag Feature

Reduce lag by disabling unnecessary visual effects:

```lua
_G.AntiLag = true          -- Removes animations, projectiles, and enemies visually
```

This improves performance on slower computers without affecting gameplay!

---

## Discord Webhooks (Optional)

Webhooks let the script send you updates to Discord when matches finish!

### Setup

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

### Getting Your Webhook URL

1. Go to your Discord server
2. Right-click a channel → Edit Channel → Integrations → Webhooks
3. Create a new webhook and copy the URL
4. Paste it into the `_G.Webhook` setting

### What You'll Receive

When webhook notifications are enabled, you'll get Discord messages showing:
- Match result (WIN or LOSS)
- Coins and gems earned
- Bonus items collected
- Session totals
- Time completed
- Current level and wave reached

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

### "No HTTP Function" Error

**Problem:** Script fails with "No HTTP Function" message

**Solution:** Your executor doesn't support HTTP requests. Use one of these instead:
- Wave
- Xeno
- Delta
- Arceus X

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

### Multiplayer Not Connecting

**Problem:** Players can't join your multiplayer game

**Solution:** 
1. Make sure all players use the **exact same party code**
2. The host must have VIP to select maps
3. All players must use compatible difficulty settings
4. Check that everyone is in the same lobby screen at the same time

---

## Quick Start Example

Here's a simple complete example to get you started:

```lua
-- ===== CONFIGURATION =====
_G.AutoStrat = true
_G.AutoSkip = false
_G.AutoPickups = true

-- ===== LOAD LIBRARY =====
local TDS = loadstring(game:HttpGet("https://raw.githubusercontent.com/DuxiiT/auto-strat/refs/heads/main/Library.lua"))()
TDS:Addons()

-- ===== LOBBY SETUP =====
TDS:Loadout("Mercenary Base", "Military Base", "Ranger", "Accelerator", "Engineer")
TDS:Mode("Hardcore")
TDS:GameInfo("Simplicity", {})

-- ===== STRATEGY (IN-GAME) =====
TDS:Place("Mercenary Base", 0.7064, 1.8626, 42.2678) -- 1
TDS:Place("Military Base", 5.2, 1.8626, 35.5) -- 2
TDS:Place("Ranger", 10.1, 1.8626, 28.0) -- 3
TDS:Place("Accelerator", 15.3, 1.8626, 20.5) -- 4
TDS:Place("Engineer", 20.5, 1.8626, 13.0) -- 5

-- Upgrade towers
TDS:Upgrade(1)
TDS:Upgrade(2)
TDS:Upgrade(3)

-- Set targeting
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
| `_G.AntiLag` | bool | `true` | Reduce lag |
| `_G.SendWebhook` | bool | `true` | Send match results to Discord |
| `_G.Webhook` | string | `""` | Your Discord webhook URL |
| `_G.AntiLag` | bool | `true` | Reduce lag |

---

## CREDITS TO:

TDS AutoStrat BIG CREDITS TO DuxiiT https://github.com/DuxiiT/auto-strat/tree/main

**Questions?** Make sure to check the examples above. If you're still confused about a concept, it's probably explained earlier in this guide!
