# Instant Arena

A production-style 3D first-person shooter prototype built with Three.js. Face off against an AI opponent in an enclosed arena with full mouse/keyboard controls, dual weapons, and multiple difficulty levels.

## Features

### Core Gameplay
- **3-Life Elimination System**: Both you and the enemy have 3 lives. First to deplete all opponent lives wins.
- **Round-Based Combat**: Lose a life, respawn with brief invulnerability. Arena resets between rounds.
- **Dual Weapon System**:
  - **Pulse Carbine** (1/Q): Fast fire rate, moderate damage, high accuracy
  - **Heavy Bolter** (2/E): Slow fire rate, massive damage, explosive impact sound
- **3 Difficulty Levels**: Rookie, Veteran, Hardcore - each affecting enemy speed, accuracy, fire rate, and damage

### Controls
| Input | Action |
|-------|--------|
| Mouse | Look around (pitch/yaw) |
| W / Up Arrow | Move forward |
| S / Down Arrow | Move backward |
| A | Strafe left |
| D | Strafe right |
| Left Click / Space | Fire weapon |
| 1 / Q | Switch to Pulse Carbine |
| 2 / E | Switch to Heavy Bolter |
| Enter | Restart after match ends |

### Production Features
- **Mouse Look**: Full pointer-lock FPS controls with pitch limits
- **Damage Feedback**: Screen flash + directional hit indicators (front/back/left/right)
- **Enemy Barks**: Audio callouts when enemy engages, takes damage, or dies
- **Ambient Audio**: Low atmospheric drone during matches
- **Persistent Leaderboard**: Top 10 scores saved to localStorage with difficulty tracking
- **Visual Polish**: Hit markers, muzzle flashes, pulsing enemy effects, radar/minimap
- **Collision System**: Player and bullets collide with arena obstacles
- **Spawn Shields**: Brief invulnerability on respawn to prevent spawn camping

## Quick Start

### Prerequisites
- Modern web browser with WebGL support
- Local web server capability

### Running the Game

```bash
cd /path/to/Instant
python3 -m http.server 8000
```

Then open: `http://localhost:8000/index.html`

Click **"Deploy To Arena"** to start. The game will request pointer lock for mouse control.

### Alternative Servers
Any static file server works:
```bash
# Node.js
npx serve .

# PHP
php -S localhost:8000

# Python 2
python -m SimpleHTTPServer 8000
```

## Technical Architecture

### Stack
- **Runtime**: Modern web browser
- **Renderer**: Three.js (r163 via CDN)
- **Audio**: Web Audio API (procedural generation)
- **Storage**: localStorage (leaderboard persistence)
- **Build**: None - single-file architecture

### File Structure
```
Instant/
├── index.html      # Complete game (HTML/CSS/JS)
├── README.md       # This file
├── SPECS.md        # Detailed technical specification
└── LICENSE         # License file
```

### Code Organization
The single-file architecture keeps everything self-contained:

1. **CSS Variables**: Theming system for consistent colors
2. **Three.js Setup**: Scene, camera, renderer, lighting
3. **Arena Generation**: Floor, walls, obstacles with collision data
4. **Entity Systems**: Player and enemy with shared damage/respawn logic
5. **AI State Machine**: PUSH → ENGAGED → RETREAT based on distance
6. **Audio System**: Layered procedural sound effects
7. **HUD/DOM**: Real-time stat updates, radar rendering
8. **Input Handling**: Keyboard + pointer-lock mouse

## Game Systems

### Enemy AI Behavior
The opponent uses a finite state machine with three states:

- **PUSH** (distance > 11): Closes gap aggressively, occasional bark
- **ENGAGED** (6.5-11): Optimal fighting range, strafing fire
- **RETREAT** (distance < 6.5): Backs away to ideal range

Strafe direction switches randomly every 0.7-1.9 seconds for unpredictability.

### Difficulty Scaling
| Stat | Rookie | Veteran | Hardcore |
|------|--------|---------|----------|
| Speed | 7 | 9.5 | 11 |
| Fire Rate | 0.8s | 0.55s | 0.4s |
| Accuracy | 0.08 spread | 0.05 spread | 0.03 spread |
| Damage | 12 | 14 | 18 |

### Damage Model
- **Falloff**: Damage decreases with distance (closer = more damage)
- **Weapon Multiplier**: Heavy Bolter deals 1.6x base damage
- **Headshot Bonus**: None (cylindrical hitboxes for simplicity)

### Audio Architecture
All sounds generated procedurally via Web Audio API:

- **Player Shot**: Sawtooth sweep + click transient
- **Enemy Shot**: Lower pitch, shorter duration
- **Hit Confirm**: Triangle wave with pitch drop
- **Impact**: Filtered noise burst
- **Enemy Bark**: Sawtooth with state-appropriate pitch contour
- **Ambient**: 55Hz sine wave drone

## Browser Support

Tested and working:
- Chrome 120+
- Firefox 121+
- Edge 120+
- Safari 17+ (WebGL 2.0)

Requires:
- WebGL 2.0 support
- ES6 modules
- Pointer Lock API
- Web Audio API

## Development

### No Build Step
This is intentionally a zero-build project. Edit `index.html` directly:

```bash
# Edit with your preferred editor
code Instant/index.html

# Serve and test
python3 -m http.server 8000
```

### Key Variables for Tuning
```javascript
// Player stats
player.speed = 13.5;        // Movement speed
player.fireCooldown = 0.22; // Seconds between shots (primary)

// Enemy stats (hardcore default)
enemy.speed = 11;
enemy.fireCooldown = 0.4;
enemy.accuracy = 0.03;  // Radians of spread

// Arena
arenaSize = 58;
worldBounds = arenaSize * 0.5 - 1.2;
```

### Adding Features
The modular function structure makes it easy to extend:

```javascript
// Add a new weapon
function fireSpecialWeapon() {
  // Create projectile
  // Apply unique physics
  // Trigger custom audio
}

// Add a pickup
function spawnHealthPickup() {
  // Create mesh at random position
  // Check collision with player
  // Apply healing effect
}
```

## Performance Notes

- **Target**: 60 FPS on mid-range hardware
- **Optimizations**:
  - Frustum culling (Three.js default)
  - Object pooling for bullets (cleanup on expiration)
  - Delta-time clamping (prevents large time steps)
  - Material disposal on bullet cleanup
  - Single canvas for radar (2D context, not WebGL)

## Known Limitations

- No mobile/touch support (requires keyboard + mouse)
- Single arena layout (no procedural generation)
- No multiplayer (AI opponent only)
- No save/resume mid-match
- Audio requires user interaction first (browser policy)

## Roadmap Ideas

- [ ] Weapon pickups scattered in arena
- [ ] Destructible cover elements
- [ ] Enemy variants (sniper, rusher, tank)
- [ ] Arena hazards (laser grids, explosive barrels)
- [ ] Match replays with download
- [ ] Multiplayer via WebRTC
- [ ] VR support (WebXR)

## Credits

Built with:
- [Three.js](https://threejs.org/) - 3D graphics
- Google Fonts (Orbitron, Rajdhani) - Typography
- No external assets - all procedural geometry

## License

MIT License - See LICENSE file for details.
