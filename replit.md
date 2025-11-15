# Minecraft Bot for Aternos Server

## Overview
This is a highly optimized Minecraft bot built with Node.js and mineflayer that connects to Aternos servers. The bot simulates human-like gameplay with randomized exploration, varied activities, natural timing, and anti-detection features for 24/7 operation. **Now includes automatic creative mode switching and enhanced anti-AFK to prevent server kicks.**

## Recent Changes
- **November 15, 2025**: CRITICAL UPDATE - Auto Creative Mode Switching & Enhanced Anti-AFK
  - **Automatic creative mode switching**: Bot now switches itself to creative mode using `/gamemode creative` command
  - **Gamemode monitoring system**: Continuously monitors gamemode every 5 seconds and auto-corrects if changed
  - **Player update event listener**: Immediately detects when admins change bot's gamemode and auto-corrects
  - **Enhanced anti-AFK system**: Reduced check interval from 15s to 8s for better coverage
  - **More anti-AFK actions**: Added 4 new actions (forward/back movement, arm swing, yaw rotation) for total of 9 actions
  - **Improved AFK threshold**: Reduced from 30-120s to 15-45s random intervals for more frequent activity
  - **Better sleep detection**: Added isSleeping check to anti-AFK to prevent actions during sleep
  - **Enhanced bed placement**: Tries 4 different directions with better error handling
  - **Smart bed finding**: Shows distance to found bed and better logging
  - **Auto creative mode on spawn**: Detects non-creative mode and auto-switches within 3 seconds
  - **Physics tick improvements**: Increased look-around probability from 0.001 to 0.002
  - **New state variables**: `gamemodeCheckInterval`, `lastGamemode` for tracking
  - **Cleanup improvements**: Clears gamemode monitoring interval on reconnect
  - Tested and verified: Bot automatically switches to creative and stays active without kicks

- **November 15, 2025**: Added Creative Mode Inventory Support
  - **Creative mode detection**: Bot automatically detects game mode on spawn (Survival, Creative, Adventure, Spectator)
  - **Automatic item acquisition**: Bot can grab items from creative inventory when in creative mode
  - **Smart bed management**: Automatically gets beds from creative inventory when night falls
  - **Building block acquisition**: Grabs blocks (dirt, stone, etc.) from creative inventory as needed
  - **Fallback system**: Gracefully falls back to survival mode inventory management when not in creative
  - **New functions added**: `isCreativeMode()`, `getItemFromCreativeInventory()`, `ensureInventoryItem()`, `ensureBedInInventory()`
  - **Enhanced logging**: Shows game mode on spawn and creative mode status
  - **Uses bot.creative.setInventorySlot() API**: Proper creative inventory window interaction
  - Tested and verified: Bot successfully detects game mode and adapts inventory management

- **November 15, 2025**: MAJOR UPDATE - Complete bot optimization for human-like behavior and anti-detection
  - **Complete rewrite of bot.js** (~640 lines) to simulate a real player instead of robotic patterns
  - **Randomized exploration system**: Replaced perfect circles with natural random wandering (variable angles, distances, jitter)
  - **Human-like timing**: All actions use randomized delays (500ms-8s) instead of fixed intervals
  - **Natural head/camera movements**: Implemented bot.look() with random yaw/pitch rotations while walking and performing actions
  - **Varied activity system**: Bot randomly chooses between explore, build, idle, and interact activities with weighted probabilities
  - **Movement jitter**: Added 1-3 block imperfection to all movements to avoid straight-line patterns
  - **Anti-AFK prevention system**: Performs random actions (jumps, crouches, inventory checks, looking around) every 30-120 seconds
  - **Random action execution**: 5 different human-like behaviors (jumping, crouching, looking, standing still, item swapping)
  - **Activity variation**: Different behaviors during day/night cycles, no repetitive patterns
  - **Natural block placement**: Randomizes placement direction order, uses 1-3s delays between placing and breaking
  - **Updated config.json**: New settings with exploreRadius (25 blocks), removed rigid circle settings, added buildingEnabled flag
  - **Fixed anti-AFK interval stacking**: Properly clears interval on reconnect to prevent multiple overlapping timers
  - **24/7 operation optimized**: Robust reconnection handling with randomized delays
  - **Anti-pattern detection**: All timing, movement, and actions fully randomized to avoid bot detection
  - Tested and verified: Bot exhibits natural player-like behavior without detectable patterns

## Project Architecture

### Structure
```
.
├── server.js           # Health check server and bot supervisor
├── bot.js              # Main bot logic with auto creative mode switching
├── config.json         # Location and behavior configuration
├── package.json        # Node.js dependencies
├── .env.example        # Example environment variables
├── .env                # Actual environment variables (not in git)
├── .gitignore          # Git ignore patterns
└── README.md           # Documentation
```

### Key Components
1. **server.js**: Health check server and process supervisor containing:
   - Express HTTP server that binds to port 3000 (required for Render deployment)
   - Health check endpoints at /health and / for monitoring
   - Bot process spawning and supervision
   - Automatic bot restart on crashes
   - Graceful shutdown handling for SIGTERM and SIGINT

2. **bot.js**: Main application file containing:
   - Minecraft server connection logic (reads from environment variables)
   - **Automatic creative mode switching**: Uses `/gamemode creative` command
   - **Gamemode monitoring**: Checks every 5 seconds and auto-corrects
   - **Player update event listener**: Detects gamemode changes instantly
   - **Enhanced anti-AFK system**: 9 actions, 8s check interval, 15-45s thresholds
   - **Creative mode inventory management**: Detects creative mode and enables full inventory access
   - **Automatic item acquisition**: Gets items from creative inventory as needed
   - Human-like behavior utilities (randomDelay, randomFloat, randomChoice, shouldDoActivity)
   - Natural head movement simulation with bot.look() API
   - Random action execution system (jumps, crouches, inventory checks, looking around, arm swing, movement)
   - Anti-AFK prevention system with randomized timing and sleep detection
   - Activity-based gameplay (explore, build, idle, interact) with weighted random selection
   - Randomized exploration with variable angles, distances, and movement jitter
   - Natural block placement with random direction order and human-like delays
   - Enhanced bed finding with distance logging and 4-direction placement attempts
   - Chest interaction with random deposit/withdraw behaviors
   - Day/night cycle adaptation with automatic sleep functionality
   - Event handlers for bot lifecycle and reconnection with proper cleanup

3. **config.json**: Configuration for:
   - Exploration radius (how far bot wanders from spawn)
   - Block type to place/break (must be in inventory for survival mode)
   - Block breaking permissions (canDig setting)
   - Building activity toggle (buildingEnabled)
   - Auto-sleep during night time
   - Required inventory items (for reference, not enforced in creative mode)
   - Chest interaction settings (deposit/withdraw items)

### Dependencies
- `express`: HTTP server for health checks and Render compatibility
- `mineflayer`: Core Minecraft bot framework
- `mineflayer-pathfinder`: Navigation and pathfinding
- `dotenv`: Environment variable management

## Automatic Creative Mode Features

### How It Works
The bot uses multiple layers to ensure it stays in creative mode:

1. **On Spawn Check**: When bot spawns, checks `bot.player.gamemode` and auto-switches if not creative (3s delay)
2. **Gamemode Monitoring**: Every 5 seconds, checks current gamemode and sends `/gamemode creative` if changed
3. **Player Update Events**: Listens to `playerUpdated` event and immediately corrects if gamemode changes (2s delay)
4. **Command Execution**: Uses `bot.chat('/gamemode creative')` to switch modes

### Functions
- `ensureCreativeMode()`: Checks current gamemode and sends switch command if needed
- `startGamemodeMonitoring()`: Sets up 5-second interval to monitor and auto-correct gamemode
- `playerUpdated` event listener: Immediately detects and corrects gamemode changes

### State Variables
- `lastGamemode`: Tracks previous gamemode to detect changes
- `gamemodeCheckInterval`: Interval ID for gamemode monitoring
- Both cleared on reconnect to prevent multiple overlapping monitors

## Enhanced Anti-AFK Features

### Improvements
- **Check interval**: Reduced from 15s to 8s for better coverage
- **Action threshold**: Reduced from 30-120s to 15-45s for more frequent activity
- **New actions**: Added forward/back movement, arm swing, yaw rotation (total 9 actions)
- **Sleep detection**: Won't perform anti-AFK actions while sleeping
- **Physics tick**: Increased look-around probability from 0.001 to 0.002

### How It Prevents Kicks
- Random actions every 15-45 seconds
- Check runs every 8 seconds
- 9 different human-like behaviors
- Updates lastActivityTime on every action
- Physics tick random looks every ~500 ticks
- No actions during sleep (prevents wake-up issues)

## Configuration Required

### Environment Variables
Users need to set up these environment variables:
- `MINECRAFT_HOST`: Aternos server address
- `MINECRAFT_PORT`: Server port (usually 25565)
- `MINECRAFT_USERNAME`: Microsoft account email for online mode, or any username for offline mode
- `MINECRAFT_VERSION`: Minecraft version (e.g., 1.20.1)
- `MINECRAFT_AUTH`: Authentication mode ('microsoft' for online servers, 'offline' for cracked servers)

### Server Requirements
- Aternos server must be running
- **Bot needs OP permissions** or gamemode permission to run `/gamemode creative` command
- Bot account may need to be whitelisted
- Server must allow block placement/breaking

## Getting Started on Replit

### Quick Start
1. **Create your .env file**: Copy `.env.example` to `.env` and fill in your Aternos server details
   ```bash
   cp .env.example .env
   ```
   Then edit `.env` with your server information:
   - `MINECRAFT_HOST`: Your Aternos server address (e.g., craftpixel-R1dt.aternos.me)
   - `MINECRAFT_PORT`: Your server port (shown on Aternos dashboard when server is running)
   - `MINECRAFT_USERNAME`: Your Microsoft account email (for online mode)
   - `MINECRAFT_VERSION`: Your server's Minecraft version
   - `MINECRAFT_AUTH`: Set to `microsoft` for Aternos servers

2. **Start your Aternos server**: Make sure your server is online before running the bot

3. **Give bot OP permissions**: Once bot connects, run in-game: `/op YourBotUsername`
   - This allows the bot to run `/gamemode creative` command
   - Alternatively, give gamemode permission in your permissions plugin

4. **Run the bot**: Click the "Run" button or restart the workflow
   - The bot will display a Microsoft authentication link if needed
   - Follow the on-screen instructions to authenticate
   - Once authenticated, the bot will connect and **automatically switch to creative mode**
   - You'll see: "⚠️  Detected Survival mode - switching to Creative..."

### Important Notes
- **Bot automatically switches to creative mode** - no manual `/gamemode` needed!
- The bot continuously monitors gamemode and auto-corrects if changed
- **Bot needs OP or gamemode permissions** to run `/gamemode creative` command
- Enhanced anti-AFK system prevents kicks (8s checks, 15-45s action intervals)
- Add the bot to your server whitelist if enabled
- The bot will automatically reconnect if disconnected (up to 10 attempts)
- Authentication is cached in the `auth-cache` folder - you only need to authenticate once

## Usage
1. Configure `.env` with server details
2. Edit `config.json` with desired settings
3. Start Aternos server
4. **Give bot OP**: `/op BotName` (required for auto creative mode)
5. Run `npm start`
6. Bot automatically switches to creative mode and starts playing!

## User Preferences
- Keep automatic creative mode switching enabled by default
- Maintain enhanced anti-AFK system for kick prevention
- Preserve human-like randomized behavior for anti-detection
- Keep 24/7 operation capability with auto-reconnect

## Deployment
The bot is designed to be deployed on Render.com by connecting a GitHub repository. See README.md for detailed deployment instructions.
