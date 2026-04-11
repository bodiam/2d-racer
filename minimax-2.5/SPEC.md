# 2D Top-Down Racing Game Specification

## Project Overview
- **Project Name**: 2D Racer
- **Project Type**: Browser-based 2-player hotseat racing game
- **Core Functionality**: A top-down racing game where two players compete in rounds of 3 laps on randomly generated tracks
- **Target Users**: Casual gamers looking for local multiplayer fun

## UI/UX Specification

### Layout Structure
- **Canvas Size**: 800×600 pixels, centered on page
- **Background**: Dark charcoal (#1a1a2e) outside canvas area
- **Game States**: Menu → Countdown → Racing → Round End → Race End

### Visual Design

#### Color Palette
- **Grass/Off-track**: #3d5c3d (muted green)
- **Road Surface**: #2a2a2a (dark asphalt)
- **Road Edge**: #1a1a1a (darker asphalt)
- **Lane Markings**: #ffffff (white dashed lines)
- **Start/Finish Line**: #ffffff and #000000 (checkered pattern)
- **Player 1 Car**: #e63946 (bright red)
- **Player 2 Car**: #457b9d (steel blue)
- **UI Text**: #ffffff with #000000 outline
- **UI Background**: rgba(0, 0, 0, 0.7) semi-transparent panels

#### Typography
- **Primary Font**: System default sans-serif (Arial/Helvetica)
- **HUD Text Size**: 18px bold
- **Countdown/Title Text Size**: 72px bold
- **Round End Text Size**: 48px bold

#### Spacing
- **Canvas Padding**: 20px margin from edges
- **HUD Padding**: 10px from canvas edges
- **Car Size**: 30px width × 20px height

### Components

#### Track Generation
- 8 waypoints placed around a center point (400, 300)
- Waypoints at random distances (100-250px from center)
- Sorted by angle to create proper loop
- Catmull-Rom spline interpolation for smooth curves
- Road width: 80px
- Track fully contained within canvas with 40px buffer

#### Cars
- Rectangular shape with rounded corners
- Red car (P1) starts on left side of start line
- Blue car (P2) starts on right side of start line
- Subtle shadow underneath for depth

#### HUD Elements
- **Top Bar**: P1 laps (red), P2 laps (blue), Round indicator (white)
- **Round Wins Display**: Row of 3 dots per player showing round wins
- **Bottom Controls**: "← → = P1 | A D = P2" small text

#### Game States Screens
- **Countdown**: Large "3", "2", "1", "GO!" in center
- **Round End**: "P1 Wins Round!" or "P2 Wins Round!" with pause
- **Race End**: Winner announcement with total round wins

## Functionality Specification

### Core Features

#### Track Generation Algorithm
1. Generate 8 waypoints at random angles around center
2. Random distance from center (100-250px, minimum 40px from edge)
3. Sort waypoints by angle (counterclockwise)
4. Use Catmull-Rom spline to interpolate smooth curve
5. Calculate inner and outer road edges (road width 80px)
6. Place start/finish line at first waypoint, perpendicular to track direction

#### Car Physics
- **Auto-acceleration**: Constant forward speed (150 pixels/second base)
- **Steering**: 
  - P1: Left/Right arrow keys
  - P2: A/D keys
  - Turn rate: 3.5 radians/second, proportional to current speed
- **Drift/Inertia**: 
  - Velocity lerps toward facing direction (0.92 factor)
  - Slight sideways slide during turns
- **Off-road penalty**: Speed reduced to 60% when on grass

#### Collision System
- Circle-based collision detection (radius = car half-width)
- On overlap: apply impulse to push cars apart
- Bounce velocity: 200 pixels/second in collision normal direction
- Brief invulnerability (100ms) after collision to prevent stuck

#### Lap Counting
- Invisible checkpoint line at start/finish
- Must cross in correct direction (forward along track)
- Lap increments when crossing after completing previous lap
- Direction check: car must be within 45° of track direction

#### Race Structure
- 3 laps per round
- 3 rounds total
- Track regenerated between rounds
- 3-second countdown before each round ("3-2-1-GO!")
- 3-second pause after each round end showing winner

### Audio (Web Audio API)

#### Engine Sound
- Base oscillator: sawtooth wave at 80Hz
- Frequency modulated by car speed (80-200Hz range)
- Gain: 0.1 (subtle background)
- Separate instance per car, mixed together

#### Collision Sound
- White noise burst, 100ms duration
- Bandpass filter at 200Hz
- Gain envelope: quick attack, medium decay

#### Lap Complete Sound
- Sine wave beep at 880Hz (A5)
- 100ms duration
- Quick attack/decay

#### Round Win Fanfare
- Arpeggio: C-E-G-C (one octave up)
- 150ms per note, 600ms total
- Sine waves, gain 0.15

#### Countdown Beeps
- 3-2-1: 440Hz (A4) beeps, 150ms each
- GO!: 880Hz (A5) higher beep, 300ms
- Quick attack/decay envelope

### User Interactions
- **Keyboard Controls**:
  - P1: Left Arrow (turn left), Right Arrow (turn right)
  - P2: A (turn left), D (turn right)
- **Start Game**: Automatic on page load
- **Continue**: Automatic after round/race end delays

### Edge Cases
- Cars at exactly same position: separate by random offset
- Car stuck outside track: push toward track center
- Both cars finish lap same frame: both count
- Page loses focus: pause game (optional)

## Technical Implementation

### Code Structure (Single HTML File)
```
1. CSS Styles (in <style>)
2. HTML Canvas Element
3. JavaScript:
   - Constants/Configuration
   - Global State
   - Audio System
   - Track Generation
   - Car Class
   - Collision Detection
   - Game Loop
   - Rendering
   - UI Drawing
   - Input Handling
   - Game State Management
```

### Rendering Approach
- Clear canvas each frame
- Draw grass background
- Draw road (filled polygon)
- Draw lane markings (dashed lines)
- Draw start/finish line
- Draw cars with shadows
- Draw HUD overlay

## Acceptance Criteria

### Visual Checkpoints
- [ ] Track is smooth closed loop, fully visible in canvas
- [ ] Road has clear edges, lane markings visible
- [ ] Start/finish line has checkered pattern
- [ ] Both cars visible with distinct colors
- [ ] HUD shows lap counts, round number, round wins
- [ ] Countdown displays clearly before each round
- [ ] Round end shows winner clearly

### Functional Checkpoints
- [ ] Track randomizes each round
- [ ] Cars auto-accelerate forward
- [ ] P1 controls (arrows) work correctly
- [ ] P2 controls (A/D) work correctly
- [ ] Cars drift/slide during turns
- [ ] Cars slow down on grass
- [ ] Cars bounce off each other on collision
- [ ] Lap counting works correctly
- [ ] Round advances after 3 laps
- [ ] Race ends after 3 rounds with winner screen
- [ ] All sounds play at appropriate times

### Audio Checkpoints
- [ ] Engine sound loops and varies with speed
- [ ] Collision makes thud sound
- [ ] Lap complete makes beep
- [ ] Countdown makes beeps (3-2-1-GO)
- [ ] Round win plays fanfare

### Technical Checkpoints
- [ ] Single HTML file, no external dependencies
- [ ] Works in Chrome, Firefox, Safari, Edge
- [ ] No console errors during gameplay
- [ ] Smooth 60fps gameplay