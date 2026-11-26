# ⚡ Pokémon: Evolution Expedition

*A cozy Game Boy Advance adventure. CS 2261 (Media Device Architecture) final project, Spring 2022.*

Two sleepy Eevees are waiting at home. Guide **Pichu** out into the world, brave a volcano and a forest to find their **evolution stones**, and bring them back to watch your friends evolve.

There's no game over here: if Pichu gets hurt, it's sent home to try again. The game is meant to be positive, completion-based, and cute.

---

## 🎮 How to play

| Button | Action |
| --- | --- |
| **D-Pad** | Move Pichu |
| **START** | Begin the game / continue from the instructions |
| **SELECT** | Pause |
| **A** or **START** | Skip a "you got hurt" / "evolved!" screen (after 1 second) |
| **B** *(in the forest)* | Activate the cheat (see below) |

**In the pause menu:** START resumes, SELECT shows the instructions again, and B returns to the title screen.

### The journey

1. **🏠 Town:** you start just outside the house, where your two Eevees are napping. Walk up through the doorway to visit them, and out the bottom to leave.
2. **🌋 The Volcano (Fire Stone):** take the path on the **west** side of town. Dodge the falling lava rocks, keep your feet out of the lava pools, and find the cave at the bottom. The Fire Stone inside evolves an Eevee into **Flareon**.
3. **🌲 The Forest (Leaf Stone):** take the path on the **east** side of town. A Togekiss flies overhead and drops... *surprises*. Keep walking right to reach the clearing, where the Leaf Stone evolves an Eevee into **Leafeon**.
4. **🏆 Win:** collect both stones and celebrate with your evolved friends!

> **💡 Cheat:** press **B** in the forest to give Pichu cover overhead that blocks anything dropped from above.

---

## ✨ Features

| Requirement | How it's done |
| --- | --- |
| **Tiled game state** | All gameplay runs in **Mode 0** with scrolling tile backgrounds |
| **Sprites** | Animated Pichu, animated sleeping and evolved Eevees, Togekiss, lava rocks, droppings, and the cheat cover |
| **Parallax** | The forest's sky layer scrolls slower than the foreground |
| **Runtime palette changes** | The volcano's palette cycles every frame to shimmer with heat |
| **States** | Start, Instructions, Game, Pause, and Win |
| **Collision** | Collision maps for walls, doors, lava pools, and stones, plus sprite collisions for lava rocks and droppings |
| **Timers and interrupts** | Timer interrupts count seconds for the "you got hurt" and "you evolved an Eevee" screens |
| **Sound** | Two digital sound channels at once: looping music for each area, plus a one-shot evolution jingle |
| **Cheat** | Overhead cover in the forest that changes how the hazard works, instead of just skipping it |

---

## 🛠️ Building and running

You'll need a GBA emulator such as [mGBA](https://mgba.io/). Then open `Project.gba` in it:

```sh
open -a mGBA Project.gba        # macOS
```

To rebuild the ROM from source, use either option:

- **Docker** (the course toolchain image; also the `build` task in `.vscode/tasks.json`):
  ```sh
  docker run --rm -it -v "${PWD}:/gba" aaaronic/gba-compiler:1.2
  ```
- **devkitPro** installed at `/opt/devkitpro`:
  ```sh
  make
  ```

### Project layout

| File(s) | Purpose |
| --- | --- |
| `main.c` | Top-level state machine (start, instructions, game, pause, win) |
| `game.c` / `game.h` | Player movement, camera, stages, hazards, and stone pickups |
| `sound.c` / `sound.h` | Digital sound playback and timer interrupts |
| `mode0.c` / `mode0.h` | GBA hardware helpers (DMA, sprites, collision, input) |
| `*CM.c` | Collision maps for each area |
| `*.c` from `.bmp` / `.wav` | Backgrounds, sprites, and music, converted to C arrays |

---

## 📜 Development history

<details>
<summary><b>Final submission checklist</b></summary>

- **At least 4 unique sprites, 2 animated:** player (animated), sleeping Eevee, standing Eevee, and lava blobs.
- **Background requirement:** parallax movement in the forest level.
- **Modify tiles or palette at runtime:** palette changes in the volcano level.
- **START, INSTRUCTIONS, GAME, PAUSE states:** all implemented, with the game in Mode 0.
- **WIN state:** reached when both evolution stones are collected.
- **Meaningful collision:** doors, lava pools, lava rocks, and evolution stones.
- **Timers and interrupts:** used for the "you got hurt" and "you evolved an Eevee" screens.
- **Two simultaneous sounds, one digital, one looping and one not:** area music loops, and the evolution jingle plays once.
- **A cheat:** press B in the forest.
- **Playable from the in-game instructions alone, bug-free, multiple .c/.h files, and a recorded demo.**

</details>

<details>
<summary><b>Milestone 4</b></summary>

- Every state has its own background.
- All sprites have real artwork: the animated player, sleeping and standing Eevees (later animated in the house), and lava blobs.
- **Sounds:** one music track for the whole game at this point, plus a collision sound. More were added for the final.
- **Runtime palette changes:** the volcano palette is shifted every iteration to give the illusion of heat.
- **Timer:** planned for the volcano, so the level ends when Pichu overheats. It didn't make this milestone.

</details>

<details>
<summary><b>Milestone 3</b></summary>

**TA feedback:** fix the black areas at the background edges. Implement all quest regions with their mechanics, the stones, a win condition once all stones are collected, and an indicator of collected stones.

**Progress:**
- The black areas are drawn into the images on purpose. Screen scrolling now works, but the main screen still had a thick black outline to remove next milestone.
- The volcano was rebuilt with tiles.
- The forest is mainly a visual experience with simple left/right walking. A stone shows movement for now.
- The ocean scrolled automatically, with longer loops and obstacles planned. It was later cut from the final game.
- The stones are in place, and collecting all of them ended the game.
- The inside of the house displays correctly, with Eevee interactions coming next.
- A foreground overlay showing collected stones was planned.
- There's deliberately no lose condition, to keep the game positive and cute.

</details>

<details>
<summary><b>Milestone 2</b></summary>

**TA feedback (from M1):** a great idea! Think through the inventory UI and stone limits. The background requirement needs two backgrounds moving on screen at once. Focus on core mechanics: navigating the town, entering the house, and reaching at least one quest area. Suggested starting with the Fire Stone region.

**Progress:**
- The player (a placeholder Pikachu at this point) moves in all directions.
- The player can enter the house through its entrance. The room's exit was temporarily the vase in the top-left corner.
- One quest area, the lava room, could be reached via the bottom path.
- The lava room had the Fire Stone (a red square) in the top-left corner, with lava falling straight down.
- Touching the stone teleports you outside, and the stone stays gone afterward.
- Touching a lava blob sends you back to the main screen.

Thank you for the feedback on M1 and all the help during office hours! 💛

</details>
