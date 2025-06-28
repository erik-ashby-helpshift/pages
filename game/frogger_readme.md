# Frogger Game v1.0

A classic Frogger-style arcade game built in HTML5 Canvas and JavaScript.

## File Location
`/Users/erikashby/Documents/Github/pages/game/frogger.html`

## Game Overview
Help the frog safely cross busy roads and dangerous rivers to reach the lily pads on the other side.

## Gameplay Features

### Objective
- Navigate the frog from the bottom of the screen to the lily pads at the top
- Avoid getting hit by cars on the road
- Use floating logs to cross the water safely
- Reach all 5 lily pads to advance to the next level

### Controls
- **Arrow Keys**: Move the frog up, down, left, and right
- **Up Arrow**: Move forward (awards 10 points)
- **Down/Left/Right**: Move in respective directions

### Game Mechanics

#### Scoring System
- **10 points**: Moving forward (up arrow)
- **500 points**: Reaching a lily pad
- **1000 points**: Completing a level (all lily pads occupied)

#### Lives System
- Start with 3 lives
- Lose a life when:
  - Hit by a car
  - Fall in water without being on a log
  - Drift off screen while on a log
  - Reach the top row without landing on an available lily pad

#### Progressive Difficulty
- Cars spawn faster each level
- Cars move faster each level
- Logs spawn slightly more frequently

### Game Areas

#### Safe Zones (Green)
- **Row 0**: Top area with lily pads (goal)
- **Row 7**: Middle safe zone
- **Row 14**: Starting position (bottom)

#### Road (Gray) - Rows 9-13
- Cars move horizontally at varying speeds
- Cars spawn from both sides randomly
- Collision with cars results in losing a life

#### Water (Blue) - Rows 1-6
- Logs float horizontally
- Player must ride logs to cross safely
- Falling in water without a log is fatal
- Being carried off-screen by a log is fatal

#### Lily Pads
- 5 lily pads located at positions: (1,0), (4,0), (7,0), (10,0), (13,0)
- Turn green when occupied
- Each can only be used once per level
- Must land exactly on a lily pad to score

## Technical Implementation

### Canvas Setup
- **Canvas Size**: 600x600 pixels
- **Grid**: 15x15 tiles
- **Tile Size**: 40x40 pixels

### Game Objects

#### Player (Frog)
- Green circle with black eyes
- Position tracked in grid coordinates
- 30-pixel diameter

#### Cars
- Red rectangles
- Spawn randomly on road lanes
- Move at increasing speeds per level
- Removed when off-screen

#### Logs
- Brown rectangles, 3 tiles wide
- Spawn on water lanes
- Carry the player when stepped on
- Move player with log movement

#### Lily Pads
- Green/light green circles
- Static positions at top row
- Track occupied status

### Game States
- **playing**: Normal gameplay
- **gameOver**: When all lives are lost

### Performance Considerations
- 60 FPS game loop using requestAnimationFrame
- Efficient collision detection
- Objects cleaned up when off-screen
- Optimized rendering with simple shapes

## Code Structure
- Single HTML file with embedded CSS and JavaScript
- Event-driven input handling
- Object-oriented game entity management
- Modular function organization

## Future Enhancement Ideas
- Sound effects
- Animated sprites
- Power-ups
- High score persistence
- Multiple frog characters
- Different level layouts
- Bonus rounds