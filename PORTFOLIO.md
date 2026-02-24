# 🎮 Portfolio Project: Instant Arena FPS

> **A Production-Grade 3D First-Person Shooter built with Three.js**
> 
> **Timeline:** 2-Year Portfolio Journey
> **Status:** Active Development

---

## 🚀 Project Overview

**Instant Arena** is a browser-based 3D FPS featuring a sophisticated AI opponent, dual-weapon system, and production-level polish. Built as a portfolio demonstration piece to showcase frontend engineering, game architecture, and systems design capabilities.

### Why This Matters to Employers

This isn't just a game—it's a **complete engineering demonstration**:

- ✅ **Single-file architecture** showcasing code organization skills
- ✅ **Procedural audio system** using Web Audio API (no external assets)
- ✅ **Finite state machine AI** with tactical decision-making
- ✅ **Collision detection & physics** in 3D space
- ✅ **Performance optimization** for 60FPS target
- ✅ **Zero external dependencies** (pure JavaScript/Three.js)

---

## 🛠 Technical Stack

| Component | Technology |
|-----------|------------|
| **Graphics** | Three.js (r163) |
| **Audio** | Web Audio API (procedural generation) |
| **Input** | Pointer Lock API + Keyboard |
| **Storage** | localStorage (leaderboard persistence) |
| **Build** | None—zero-build architecture |
| **Deployment** | Static hosting (GitHub Pages ready) |

---

## 🎯 Core Engineering Highlights

### 1. AI State Machine Architecture
```
PUSH → ENGAGED → RETREAT
    ↑              ↓
    └──────────────┘
```

The enemy AI uses a **3-state finite state machine** that makes tactical decisions based on:
- Distance to player
- Health status
- Randomized strafe patterns (0.7-1.9s intervals)

### 2. Procedural Audio System
No external sound files—all audio generated programmatically:

| Sound | Technique |
|-------|-----------|
| **Player Shot** | Sawtooth wave sweep + click transient |
| **Hit Confirm** | Triangle wave with pitch drop |
| **Enemy Barks** | Sawtooth with state-pitched contours |
| **Ambient** | 55Hz sine wave drone |

### 3. Collision & Physics
- **Bullet collision**: Ray-based intersection tests
- **Wall collision**: Boundary clipping with velocity reflection
- **Spatial partitioning**: Simple bounds checking for performance

### 4. Difficulty Scaling System
| Stat | Rookie | Veteran | Hardcore |
|------|--------|---------|----------|
| Speed | 7 | 9.5 | 11 |
| Fire Rate | 0.8s | 0.55s | 0.4s |
| Accuracy | 0.08 | 0.05 | 0.03 |
| Damage | 12 | 14 | 18 |

---

## 📊 Project Stats

| Metric | Value |
|--------|-------|
| **Lines of Code** | ~2,500 (single file) |
| **External Dependencies** | 1 (Three.js CDN) |
| **Browser APIs Used** | 5+ (WebGL, Web Audio, Pointer Lock, localStorage, Fullscreen) |
| **Supported Browsers** | Chrome 120+, Firefox 121+, Edge 120+, Safari 17+ |
| **Target FPS** | 60 FPS on mid-range hardware |

---

## 🏗 Architecture Decisions

### Single-File Design Philosophy
```
index.html (2,500 lines)
├── CSS Variables (theming)
├── Three.js Setup (scene, camera, renderer)
├── Arena Generation (procedural geometry)
├── Entity Systems (player, enemy, bullets)
├── AI Logic (finite state machine)
├── Audio System (Web Audio API)
├── HUD/DOM (real-time updates)
└── Input Handling (keyboard + mouse)
```

**Rationale:** Demonstrates ability to organize complex systems within constraints—critical skill for production environments.

### Zero-Build Approach
- No webpack, vite, or bundlers
- Direct browser execution
- Faster iteration cycles
- Easier deployment

---

## 🎓 Skills Demonstrated

### Frontend Engineering
- ✅ Advanced JavaScript (ES6+)
- ✅ DOM manipulation without frameworks
- ✅ Event-driven architecture
- ✅ Performance optimization

### 3D Graphics Programming
- ✅ Three.js proficiency
- ✅ WebGL understanding
- ✅ Scene graph management
- ✅ Lighting & materials

### Game Development
- ✅ Game loop architecture
- ✅ State management
- ✅ Physics simulation
- ✅ AI behavior trees

### Audio Engineering
- ✅ Web Audio API mastery
- ✅ Procedural sound generation
- ✅ Audio mixing & layering

### Software Architecture
- ✅ Modular function design
- ✅ Separation of concerns
- ✅ Code documentation
- ✅ Technical specification writing

---

## 📁 File Structure

```
portfolio/
├── index.html          # Complete game (HTML/CSS/JS ~2,500 lines)
├── README.md           # Technical documentation
├── SPECS.md            # Detailed architecture specification
├── PORTFOLIO.md        # This file - portfolio narrative
└── LICENSE             # MIT License
```

---

## 🎮 Quick Demo

### Play Now
```bash
# Clone the repo
git clone https://github.com/dshirmoh-sketch/portfolio.git
cd portfolio

# Start local server
python3 -m http.server 8000

# Open browser
# Navigate to: http://localhost:8000
# Click "Deploy To Arena"
```

### Controls
| Key | Action |
|-----|--------|
| Mouse | Look around |
| W/A/S/D | Movement |
| Left Click/Space | Fire |
| 1/Q | Pulse Carbine |
| 2/E | Heavy Bolter |
| Enter | Restart after match |

---

## 📈 Development Timeline

This portfolio represents a **2-year journey** of continuous learning:

### Phase 1: Foundation (2024)
- [x] Core game mechanics
- [x] Three.js integration
- [x] Basic AI opponent
- [x] Weapon system

### Phase 2: Polish (2025 Q1)
- [x] Procedural audio system
- [x] Difficulty scaling
- [x] Leaderboard persistence
- [x] Visual effects

### Phase 3: Production (2025 Q2)
- [x] Performance optimization
- [x] Cross-browser testing
- [x] Documentation
- [x] Portfolio presentation

### Future Roadmap
- [ ] Weapon pickups system
- [ ] Enemy variants (sniper, rusher, tank)
- [ ] Arena hazards
- [ ] Multiplayer via WebRTC
- [ ] WebXR/VR support

---

## 🔗 Links

- **Live Demo:** [Coming Soon - GitHub Pages]
- **Repository:** https://github.com/dshirmoh-sketch/portfolio
- **LinkedIn:** [Add your LinkedIn]
- **Email:** [Add your email]

---

## 💼 For Employers

### What This Project Proves

1. **Self-Motivated Learner** — Built complex system from scratch without tutorials
2. **Production Mindset** — Single-file constraint demonstrates real-world constraint handling
3. **Technical Communication** — Comprehensive documentation shows ability to explain complex systems
4. **Attention to Detail** — Polish elements (audio, feedback, UI) show care for user experience
5. **Full-Stack Capable** — Frontend, audio, graphics, state management—all in one

### Ideal Roles

- Frontend Engineer
- Creative Developer
- Game Developer (Web/Indie)
- Technical Artist
- Interactive Media Developer

---

## 📝 Contact


Ready to bring this level of craft to your team.

- 📧 [Your Email]
- 💼 [LinkedIn Profile]
- 🐙 [GitHub Profile]

---

## License

MIT License - See [LICENSE](./LICENSE) file for details.

---

> *"Code is like humor. When you have to explain it, it's bad."* — Cory House
> 
> This project speaks for itself. 🎮
