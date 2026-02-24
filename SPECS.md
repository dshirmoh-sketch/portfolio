# Instant Arena FPS - Product Specification

## Vision

Create a high-quality browser-based 3D first-person shooter prototype that feels like a polished vertical slice of a larger game. The player fights a single AI opponent in an arena using modern FPS controls (mouse look + WASD) with left-click firing, dual weapons, and multiple difficulty levels.

## Core Gameplay Requirements

- Genre: 3D first-person arena shooter
- Mode: 1v1 (player vs one computer opponent)
- Controls:
  - Mouse: look (pitch + yaw via pointer lock)
  - W / S: forward/backward movement
  - A / D: strafe left/right
  - Arrow keys: alternative movement
  - Left Click / Space: fire weapon
  - 1 / Q: select Pulse Carbine (primary)
  - 2 / E: select Heavy Bolter (secondary)
  - Enter: restart after match end
- Elimination rule:
  - Both player and opponent start with 3 lives
  - Each character has 100 HP per life
  - HP reaches 0 → lose one life → respawn if lives remain
  - First side reduced to 0 lives loses the match

## Technical Stack

- Runtime: modern web browser
- Rendering: Three.js (r163 via CDN module import)
- Audio: Web Audio API (procedural generation)
- Storage: localStorage (leaderboard persistence)
- Architecture: single-page application (`index.html`) with in-file CSS and JavaScript
- Hosting: static local server (`python3 -m http.server`)
- No build tooling required

## Arena & Environment

- Symmetric enclosed arena with boundary walls (58 units)
- Multiple interior cover blocks (12 obstacles) for tactical movement
- Fog and atmospheric lighting for depth and readability
- Neon-grid floor with animated sci-fi aesthetic
- Collision system for all arena geometry

## Player Systems

### Movement
- First-person camera with full mouse look (pointer lock)
- Pitch clamped to ±60 degrees (±π/3 radians)
- WASD + Arrow key support for movement
- Strafing with A/D keys (proper left/right relative to view)
- View bob (sway animation) during movement

### Weapons
| Weapon | Fire Rate | Damage | Speed | Spread |
|--------|-----------|--------|-------|--------|
| Pulse Carbine (1/Q) | 0.22s | 17 | 38 | 0.015 |
| Heavy Bolter (2/E) | 0.55s | 27 (1.6×) | 32 | 0.025 |

- Instant switching with cooldown (0.3s)
- Visual weapon indicator in HUD
- Different muzzle flash intensities
- Unique audio signatures per weapon

### Defense
- 100 HP per life, 3 lives maximum
- Spawn shield (1.1s) on respawn prevents spawn camping
- Collision resolution against walls and obstacles
- Damage direction indicators (front/back/left/right)
- Screen flash intensity scales with damage taken

## Opponent AI Systems

### Visual Design
The enemy is built as a multi-part menacing construct:
- Core capsule body (pink/crimson)
- Helmet with visor (armor material with cyan glow)
- Shoulder pauldrons (asymmetric armor plates)
- Backpack/power unit
- Handheld weapon with glowing barrel
- Spine spike (intimidation factor)
- Torus halo (status ring)
- Pulsing point-light aura

### Behavior States
- `PUSH` (distance > 11): Aggressive approach, 40% chance to bark
- `ENGAGED` (6.5-11): Optimal range, active strafing
- `RETREAT` (distance < 6.5): Creates space to ideal range

### AI Stats (by difficulty)
| Difficulty | Speed | Fire Rate | Accuracy | Damage |
|------------|-------|-----------|----------|--------|
| Rookie | 7 | 0.8s | 0.08 rad | 12 |
| Veteran | 9.5 | 0.55s | 0.05 rad | 14 |
| Hardcore | 11 | 0.4s | 0.03 rad | 18 |

### Animations
- Continuous pulse animation (3.2 rad/s) on visor, core, halo, aura
- Look-at tracking (always faces player)
- Strafe direction switching (0.7-1.9s intervals)
- Dynamic state transitions with thinking timer (0.5-1.1s)

## Combat & Damage Model

### Damage Formula
```
damage = base_damage × distance_falloff × weapon_multiplier

falloff = clamp(1 - distance / max_range, min_mult, 1.0)
```

### Falloff Ranges
- Player shots: falloff starts at 65 units, min 68% damage
- Enemy shots: falloff starts at 72 units, min 62% damage

### Hit Detection
- Cylindrical hitboxes (capsule approximation)
- Player radius: 1.0 unit
- Enemy radius: 0.95 unit
- Bullet radius: 0.14 unit
- Collision with all arena obstacles

### Round Flow
1. Player/enemy HP reaches 0
2. Life decremented
3. If lives remain: 1.3s delay → respawn both
4. If no lives: match end → save score → show restart

## HUD & UX

### Main HUD Panels
- **Top Left**: Player HP, lives (×3), score
- **Top Right**: Enemy HP, lives (×3), current AI state
- **Bottom Center**: HP bars (player + enemy), threat meter
- **Bottom Left**: Round number, current weapon, difficulty
- **Bottom Right**: Radar/minimap (180×180 canvas)
- **Weapon Indicator**: Bottom-right floating badge

### Feedback Elements
- Crosshair: Cyan with drop shadow
- Hit marker: Pink X burst (100ms duration)
- Combat text: Weapon switch notifications
- Damage overlay: Red radial gradient flash
- Directional indicators: Edge-of-screen arrows

### Boot Overlay
- Title and description
- Control reference list
- Difficulty selector (Rookie/Veteran/Hardcore buttons)
- Deploy button (starts match + requests pointer lock)
- Leaderboard display (top 10 scores from localStorage)

## Audio System

### Architecture
All audio generated procedurally via Web Audio API:

### Sound Types
| Sound | Waveform | Notes |
|-------|----------|-------|
| Player shot | Sawtooth + click | Pitch sweep 520→180Hz |
| Enemy shot | Sawtooth (lower) | Pitch sweep 280→120Hz |
| Player hit | Triangle | Drop 200→90Hz |
| Enemy hit | Triangle | Drop 750→450Hz |
| Impact | Filtered noise | Lowpass 800Hz |
| Enemy bark (engage) | Sawtooth | 150→120Hz |
| Enemy bark (hit) | Sawtooth | 200→80Hz |
| Enemy bark (die) | Sawtooth | 100→40Hz, long decay |
| Ambient | Sine | 55Hz drone, 0.025 gain |

### Behavior
- AudioContext resumes on user interaction (browser policy)
- Ambient starts on match begin, fades on match end
- Layered sounds for weapon variety
- Gain ramping prevents clipping

## Score Persistence

### Leaderboard System
- localStorage key: `instantArenaScores`
- Stores: score, difficulty, timestamp
- Max entries: 10 (sorted descending)
- Displayed on boot screen

### Scoring
- +100 points per enemy elimination
- Final score shown on victory
- Difficulty recorded with each entry

## Performance & Quality

### Frame Loop
- Delta-time clamping: max 33ms (prevents large time steps)
- RequestAnimationFrame with timestamp
- Separate update/render phases

### Memory Management
- Bullet cleanup on TTL expiration or hit
- Material disposal (bullet mesh + tracer)
- Scene removal for destroyed objects
- No object pooling currently (simple respawn)

### Visual Optimizations
- Single canvas for radar (2D context)
- CSS transforms for HUD (GPU accelerated)
- Three.js frustum culling enabled
- Shadow map 1024×1024 (balanced quality/performance)

## Browser Support

### Required APIs
- WebGL 2.0 (Three.js renderer)
- ES6 modules (import syntax)
- Pointer Lock API (mouse look)
- Web Audio API (procedural sound)
- localStorage (leaderboard)

### Tested Browsers
- Chrome 120+
- Firefox 121+
- Edge 120+
- Safari 17+

### Known Limitations
- No mobile/touch support (requires keyboard + mouse)
- Pointer lock requires HTTPS or localhost
- Audio requires user interaction first

## Future Expansion Roadmap

### Completed
- [x] Mouse look + WASD controls
- [x] Dual weapon system (primary/secondary)
- [x] Difficulty presets (3 levels)
- [x] Damage feedback (screen flash + directional)
- [x] Persistent leaderboard
- [x] Enhanced audio (layered SFX + ambient)
- [x] Enemy visual detail (armor + glow + aura)

### Planned
- [ ] Weapon pickups in arena
- [ ] Destructible cover
- [ ] Enemy variants (sniper, rusher, tank)
- [ ] Arena hazards (laser grids, explosive barrels)
- [ ] Match replay system
- [ ] Multiplayer via WebRTC
- [ ] Post-processing (bloom, color grading)
- [ ] VR support (WebXR)

## Acceptance Criteria

1. Player can move with WASD and look with mouse.
2. Left click fires current weapon (Space as alternative).
3. A goes left, D goes right (standard FPS convention).
4. Enemy takes damage correctly with falloff calculation.
5. Both player and enemy have exactly 3 lives.
6. Difficulty selector affects AI stats appropriately.
7. Leaderboard persists across sessions.
8. Audio feedback for all combat actions.
9. Game runs at 60 FPS on mid-range hardware.
10. Zero external dependencies beyond Three.js CDN.

## Development Notes

### File Structure
```
Instant/
├── index.html          # Complete game (1600+ lines)
├── README.md           # User documentation
├── SPECS.md            # This technical specification
└── LICENSE             # MIT License
```

### Key Constants
```javascript
arenaSize = 58              // Arena width/depth
worldBounds = 27.8         // arenaSize/2 - 1.2
player.speed = 13.5        // Units per second
mouseSensitivity = 0.002   // Radians per pixel
bullet.ttl = 2.4           // Seconds before cleanup
```

### Debug Console Commands
```javascript
// Set difficulty mid-match
currentDifficulty = 'veteran';

// Heal player
player.hp = 100;

// Spawn enemy at position
enemyMesh.position.set(0, 1.45, 0);

// Clear leaderboard
localStorage.removeItem('instantArenaScores');
```
