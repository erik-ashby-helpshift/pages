# Game Design Document: Pip's Nectar Quest (Demo Level)

**Version:** 1.0
**Date:** May 24, 2025
**Designed For:** Simple 8-bit demo using Java/HTML (interpreted as JavaScript & HTML5 Canvas)

## 1. Introduction

"Pip's Nectar Quest" is a short, single-level demo game. The player controls Pip, a small green boy, who must navigate a house-like maze to find a life-giving nectar plant. After consuming the nectar, Pip must escape the house, now patrolled by spiders he must avoid. The game emphasizes simple exploration, a clear objective shift, and an avoidance-based challenge.

## 2. Player Character: Pip

* **Name:** Pip
* **Visuals:** A small, green boy. Sprite should be simple and clearly distinguishable (e.g., 8-bit style).
* **Movement:**
    * Tile-by-tile.
    * Controlled by Arrow Keys (Up, Down, Left, Right).
    * Moves one tile per key press in the corresponding direction.
    * Cannot move through walls or obstacles.
* **Health:**
    * Starts with 3 hearts.
    * Loses 1 heart upon contact with a spider.
    * No other sources of damage in this demo.
* **Abilities:**
    * Basic movement only. No jumping, attacking, or other special abilities.

## 3. Game Environment: The House

* **Type:** A maze representing the interior of a simple house.
* **Perspective:** Top-down.
* **Size:** 15x15 tile grid.
* **Tiles:**
    * **Floor Tiles:** Basic, walkable surface (e.g., light brown).
    * **Wall Tiles:** Impassable barriers defining the maze paths (e.g., dark grey solid blocks or lines).
    * **Entrance/Exit Tile:** A specific tile on one edge of the maze (e.g., bottom-center) where Pip starts and must return to win.
* **Layout:** A mix of small "rooms" and corridors. The layout should be designed to be a solvable maze leading to a central area and then back out.
* **Visual Theme:** Simple wooden house interior.
* **Decorative Elements (Optional):** A few non-interactive 8-bit sprites (e.g., small table, pot in a dead-end) can be used sparingly to add character, if time permits. They should not obstruct movement or be interactive.

## 4. Core Gameplay Loop & Objectives

The game is divided into two main phases:

**Phase 1: Find the Nectar Plant**
1.  **Starting Point:** Pip begins at the Entrance/Exit tile.
2.  **Objective:** Navigate the maze to find the Nectar Plant located in a central area of the house.
3.  **Challenge:** Maze navigation. No enemies are present during this phase.

**Phase 2: Escape the House**
1.  **Trigger:** Occurs immediately after Pip interacts with the Nectar Plant.
2.  **Objective:** Navigate from the plant's location back to the Entrance/Exit tile.
3.  **Challenge:** Avoid the newly appeared spiders.

## 5. The Nectar Plant

* **Visuals:**
    * A unique, brightly colored 8-bit sprite (e.g., a glowing or sparkling flower – perhaps vibrant blue or yellow).
    * Should be clearly distinguishable from other game elements.
* **Location:** Placed in a somewhat central, clear location within the maze that the player must discover.
* **Interaction:**
    * Automatic when Pip moves onto the plant's tile.
    * **Effect:** Pip's health is fully restored (to 3 hearts).
    * **Feedback:** A distinct sound effect (e.g., "shimmer," "gulp") plays. The plant's sprite visually changes (e.g., wilts, stops glowing) to indicate it has been used. A brief on-screen message like "Nectar obtained!" could display for 1-2 seconds.
* **Usage:** One-time use per game attempt.

## 6. Enemies: Spiders

* **Appearance:** Triggered *after* Pip consumes the nectar.
* **Visuals:** Small, black, simple 8-bit spider sprites (e.g., 8x8 or 12x12 pixels).
* **Quantity:** 3 spiders.
* **Placement:** Spiders appear at fixed, predetermined locations along the likely escape paths from the Nectar Plant towards the Entrance/Exit. They should not block the path entirely but create a challenge requiring timing.
* **Behavior:**
    * **Movement:** Patrol a short, fixed path (e.g., back and forth over 3-4 tiles). Movement is tile-by-tile.
    * **Speed:** Approximately the same speed as Pip, or slightly slower, to allow for skillful avoidance.
    * **AI:** Non-chasing; they stick to their patrol paths.
* **Interaction with Pip:**
    * If a spider occupies the same tile as Pip (collision), Pip loses 1 heart.
    * A sound effect (e.g., "hiss," "damage thud") should play.
    * Pip cannot defeat or stun the spiders; avoidance is the only option.

## 7. Controls

* **Up Arrow:** Move Pip Up one tile.
* **Down Arrow:** Move Pip Down one tile.
* **Left Arrow:** Move Pip Left one tile.
* **Right Arrow:** Move Pip Right one tile.
* **(Optional) R Key or Button:** On "Game Over" or "Win" screen to restart the demo.

## 8. Game States & UI

* **Start Screen (Optional):** A simple screen with the game title ("Pip's Nectar Quest") and a "Start Game" prompt.
* **Gameplay Screen:**
    * Main view of the maze, Pip, plant, and spiders.
    * **Health Display:** Visually show Pip's current health (e.g., 3 heart icons in a fixed corner of the screen). Update immediately when health changes.
* **Win Condition:**
    * Pip reaches the Entrance/Exit tile *after* having consumed the nectar.
    * **Win Screen:** Display a message like "You Escaped!" or "You Win!". Option to "Play Again?".
* **Lose Condition:**
    * Pip's health drops to 0 hearts.
    * **Lose Screen (Game Over):** Display a message like "Game Over!". Option to "Try Again?".

## 9. Visuals & Art Style

* **Style:** Simple 8-bit pixel art.
* **Tile Size:** Suggested 16x16 pixels or 24x24 pixels.
    * For a 15x15 grid, this results in a canvas of 240x240 or 360x360 pixels respectively.
* **Color Palette:** Limited, clear colors.
    * Pip: Green.
    * House Walls: Dark grey or dark brown.
    * House Floor: Light brown or beige.
    * Nectar Plant: Bright, contrasting color (e.g., blue, yellow, magenta) with possible sparkle/glow effect.
    * Spiders: Black or dark grey.
* **Graphics:** Sprites for Pip, Plant (active/used states), Spider. Tileset for walls and floors.

## 10. Sound Design (Recommended)

Simple sound effects will enhance the experience. These can be basic 8-bit style sounds.
* **Pip's Movement:** Soft "blip" or "step" sound per tile moved.
* **Nectar Interaction:** "Sparkle," "shimmer," or "gulp" sound.
* **Spider Damage:** "Hiss," "thud," or short negative sound when Pip is hit.
* **Win Jingle:** Short, upbeat musical cue.
* **Lose Jingle:** Short, downbeat musical cue.

## 11. Target Platform & Technology

* **Platform:** Web browser.
* **Technology:** HTML5 (likely using the Canvas API for rendering) and JavaScript for game logic. No external game engines (e.g., Unity, Godot) are to be used.

## 12. Scope Clarification

This document outlines a *small demo level*. The focus is on implementing the core loop: maze exploration, objective interaction, introduction of a threat, and escape. Advanced features, multiple levels, complex enemy AI, or elaborate story elements are outside the scope of this demo.

## 13. Developer Notes & Implementation Considerations

* **Maze Representation:** A 2D array or similar data structure can represent the maze layout (e.g., 0 for floor, 1 for wall).
* **Collision Detection:** Simple tile-based collision (checking if the target tile for movement is a wall or, for spiders, if Pip's tile matches a spider's tile).
* **Game Loop:** A standard game loop (update game state, render graphics) will be needed. `requestAnimationFrame` is suitable for JavaScript.
* **State Management:** A simple state machine could manage game states (e.g., `START_MENU`, `PLAYING_PHASE_1`, `PLAYING_PHASE_2`, `GAME_OVER`, `WIN`).

---

This should give a developer a comprehensive understanding of what needs to be built. Let me know if you'd like any part expanded or clarified!