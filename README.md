# Survivor-Bot (ANTERO)

Survivor-Bot is a turn-based robotic game where an autonomous robot must survive in a hostile environment, collect motorcycle parts, and repair a motorcycle while avoiding and fighting zombies.

The robot operates on a grid-based board and uses sensors, decision-making logic, and pathfinding algorithms to navigate, detect threats, and achieve its objective.

## Game Objective

- The Survivor-Bot must locate and collect all required motorcycle parts.
- Collected parts must be delivered to the motorcycle.
- The game is won when all parts are delivered.
- The game is lost if the robot is caught by a zombie.

## Game Mechanics

- Turn-based gameplay:
  - One turn for the Survivor-Bot
  - One turn for the zombies
- The robot can:
  - Move 1 or 2 grid cells per turn (depending on context)
  - Perform environment recognition
  - Attack zombies
  - Pick up items (motorcycle parts or ammunition)

## Environment & Entities

### Zombies
- Represented as towers detected by the ultrasonic sensor.
- Emit a “smell” represented by colors:
  - **Red**: zombie is 1 cell away
  - **Blue**: zombie is 2 cells away

### Motorcycle Parts
- Detected as towers.
- Identified by the **green color** using the color sensor.
- Must be collected and delivered to the motorcycle.

### Ammunition
- Detected as towers.
- Identified by the **yellow color**.
- Used to attack zombies at a distance.

## Sensors & Hardware

The robot is built on the LEGO EV3 platform and uses:

- Ultrasonic Sensor — obstacle and object detection
- Color Sensor — identification of zombies, parts, and ammunition
- Touch Sensor — turn control
- Motors with tracks (instead of wheels)
- Sound output for feedback and state notifications

## Robot Behavior

### Movement
- Movement is grid-based.
- Each grid cell corresponds to a fixed movement duration.
- Gyroscope was tested but removed due to instability; movement relies on calibrated timing instead.

### Recognition
- The robot scans only the cells it can move to.
- Recognition directions depend on its current position (edges and corners limit scanning).
- Based on detected distance and color, the robot classifies objects as:
  - Zombie
  - Motorcycle part
  - Ammunition

### Combat
- If a zombie is confirmed at close range, the robot attacks.
- After attacking, the robot may be temporarily restricted in movement.
- If the robot ends a turn on a red cell while the alarm is active, it will be caught.

### Alarm System
- Activated when a motorcycle part is picked up.
- Causes zombies to stop moving randomly and start following the robot.
- Deactivated once a part is delivered to the motorcycle.

## Pathfinding & Decision Making

### Search Algorithm
- The robot uses the **A\*** search algorithm to find the fastest path to its objective.
- Heuristic used: **Manhattan distance**.

Example:
```

Robot at [3,3], goal at [5,5]
Heuristic = |3-5| + |3-5| = 4

````

### Movement Evaluation
- Possible movements include:
  - 1 or 2 cells forward, backward, left, right
  - Diagonal movements (1 cell + 1 cell)
- The robot evaluates all valid moves and selects the one with the lowest heuristic value.
- Invalid moves (outside the board or unnecessary backtracking) are ignored.

### Strategy Highlights
- Moves cautiously when zombies are nearby.
- Prioritizes speed when carrying a motorcycle part.
- Performs partial advances to disambiguate between zombies and items when necessary.
- Adjusts behavior dynamically based on detected colors and distances.

## Known Limitations

- The robot does not drop items once picked up.
- Certain rare trap scenarios can still lead to unavoidable loss.
- Large object sizes and sensor placement can affect detection accuracy.
- Tracked movement reduces turning precision compared to wheels.

## Technologies & Libraries

- Python (MicroPython)
- Pybricks for LEGO EV3


## Final Notes

Survivor-Bot demonstrates autonomous decision-making in a constrained environment, combining sensor fusion, heuristic search, and rule-based behavior to solve a dynamic survival problem.
