# Minecraft Bot with Auto Creative Mode & Enhanced Anti-AFK

A highly optimized Node.js bot that connects to your Minecraft server and simulates a real human player with natural gameplay patterns. **Now with automatic creative mode switching and enhanced anti-AFK to prevent server kicks!**

## 🚀 New Features

- ✅ **Auto Creative Mode Switching**: Bot automatically switches itself to creative mode when in survival or when changed by admins
- ✅ **Gamemode Monitoring**: Continuously monitors gamemode and auto-corrects to creative mode every 5 seconds
- ✅ **Enhanced Anti-AFK**: More frequent (8s intervals) and varied actions to prevent server kicks
- ✅ **Smart Bed Management**: Automatically finds, places, and sleeps in beds during night
- ✅ **Creative Inventory Access**: Automatically gets items from creative inventory when needed
- ✅ **Optimized Performance**: Reduced AFK kick risk from 30-120s to 15-45s action intervals

## Features

- ✅ **Automatic Creative Mode**: Bot switches itself to creative mode on spawn and when changed by others
- ✅ **Creative Mode Inventory Access**: Automatically gets items from creative inventory when in creative mode
- ✅ **Automatic Item Acquisition**: Bot grabs beds, blocks, and essential items as needed
- ✅ **Enhanced Anti-AFK**: 9 different random actions every 8 seconds to prevent kicks
- ✅ **Human-like Behavior**: Randomized exploration, natural timing, varied activities
- ✅ **Anti-Detection**: All movements, timing, and actions fully randomized
- ✅ **Natural Movement**: Random wandering with jitter, imperfect pathfinding
- ✅ **Smart Activities**: Randomly explores, builds, idles, and interacts with chests
- ✅ **Smart Bed System**: Finds nearby beds or places from creative inventory
- ✅ **Day/Night Cycle**: Automatically sleeps during night
- ✅ **24/7 Operation**: Robust reconnection handling
- ✅ **Microsoft Authentication**: Full support for online servers

## How It Works

### Automatic Creative Mode
The bot uses multiple methods to ensure it stays in creative mode:
1. **On Spawn**: Checks gamemode and auto-switches to creative if needed (3s delay)
2. **Gamemode Monitoring**: Every 5 seconds, checks if gamemode changed and auto-corrects
3. **Player Update Events**: Listens for gamemode changes and immediately corrects
4. **Command**: Sends `/gamemode creative` chat command to switch

### Enhanced Anti-AFK System
To prevent server kicks, the bot performs random actions every 15-45 seconds:
- Jump, sneak, look around
- Equip random items
- Move forward/backward
- Swing arm
- Random head rotations
- Camera movements

The anti-AFK check runs every 8 seconds (reduced from 15s) for better coverage.

### Smart Bed Management
When night falls:
1. Searches for beds within 64 blocks
2. If no bed found, gets one from creative inventory
3. Places bed in optimal location next to bot
4. Walks to bed and sleeps
5. Wakes up at dawn and resumes activities

## Setup Instructions

### 1. Configure Server Connection

Edit `.env` file with your server details:

```bash
MINECRAFT_HOST=your-server.aternos.me
MINECRAFT_PORT=25565
MINECRAFT_USERNAME=your-email@example.com
MINECRAFT_VERSION=1.20.1
MINECRAFT_AUTH=microsoft
```

**For Cracked/Offline Servers:**
```
MINECRAFT_AUTH=offline
MINECRAFT_USERNAME=BotUsername
```

### 2. Run the Bot

Click the "Run" button or execute:

```bash
npm start
```

The bot will:
1. Connect to your server
2. **Automatically switch to creative mode** (no manual `/gamemode` needed!)
3. Enable continuous gamemode monitoring
4. Start enhanced anti-AFK monitoring (8s intervals)
5. Begin exploring, building, and performing natural activities
6. Automatically find/place beds and sleep during night
7. Continue 24/7 operation without getting kicked

## Configuration

Edit `config.json` to customize:

```json
{
  "exploreRadius": 25,
  "blockType": "dirt",
  "canDig": false,
  "buildingEnabled": true,
  "autoSleep": true,
  "requiredInventory": {
    "bed": 1,
    "dirt": 64
  },
  "chestInteraction": {
    "enabled": true,
    "depositItems": {
      "cobblestone": 16,
      "stone": 16
    },
    "withdrawItems": {
      "dirt": 8
    }
  }
}
```

## Game Mode Detection

The bot automatically detects and manages your game mode on spawn:

```
✅ Bot spawned successfully!
📍 Position: X=0.0, Y=64.0, Z=0.0
🎮 Game Mode: Survival
⚠️  Not in creative mode - will auto-switch in 3 seconds...
⚠️  Detected Survival mode - switching to Creative...
🛡️  Gamemode monitoring enabled
🛡️  Enhanced anti-AFK monitoring enabled (8s interval)
```

**No manual `/gamemode creative` command needed!** The bot does it automatically.

## Anti-AFK Details

The bot performs these actions to stay active:
- **Frequency**: Every 15-45 seconds (randomized)
- **Check interval**: Every 8 seconds
- **9 different actions**: Jump, sneak, look, move, swing, etc.
- **Physics tick actions**: Random looks every ~500 physics ticks
- **Activity tracking**: Updates lastActivityTime on every action

This prevents kicks from:
- AFK timeout (usually 5-15 minutes)
- Inactivity detection
- Anti-bot plugins

## Microsoft Authentication

For servers with online mode enabled, you'll see:

```
🔐 ===== MICROSOFT AUTHENTICATION REQUIRED =====
Please open this URL in your browser:
   https://www.microsoft.com/link

Enter this code:
   ABC12DEF

Code expires in 15 minutes
==============================================
```

Follow the on-screen instructions. Authentication is cached - you only need to do this once.

## Important Notes

- ✅ **No manual gamemode switching needed** - Bot does it automatically!
- ✅ Bot works in any initial gamemode (auto-switches to creative)
- ✅ If admins change bot's gamemode, it auto-corrects within 5 seconds
- 📋 Add the bot to your server whitelist if enabled
- 🔒 The bot needs appropriate permissions to run `/gamemode` command (OP or permissions)
- 🚀 Your server must be running before starting the bot

## Troubleshooting

**Bot can't switch to creative mode:**
- Make sure the bot has OP permissions: `/op BotName`
- Or give gamemode permissions in your permissions plugin
- Check server logs for permission errors

**Bot still getting kicked for AFK:**
- Reduce `afkThreshold` in the code (currently 15-45 seconds)
- Increase anti-AFK check frequency (currently 8 seconds)
- Check server's AFK timeout settings

**Connection issues:**
- Verify your server is running and online
- Check the server address and port in `.env`
- For online servers, use `MINECRAFT_AUTH=microsoft`

## License

MIT
