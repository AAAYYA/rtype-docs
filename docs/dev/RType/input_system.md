## InputSystem.hpp

### Purpose:
The `InputSystem` class processes player input for controlling the game, including movement, shooting, and activating power-ups. It interfaces with ECS components to apply these actions to the appropriate entities.

### Includes:
```cpp
#include "InputSystem.hpp"
#include "Registry.hpp"
#include "Component.hpp"
#include "GameNetwork.hpp"
#include "AudioManager.hpp"
#include <raylib.h>
```

### File Description:
This file defines the `InputSystem`, which maps player keyboard inputs to entity actions within the game. It supports movement, firing projectiles, and power-up activation.

### Key Functions:

#### **Function: update**
```cpp
void update(float delta_time);
```
- Iterates through entities with input and velocity components.
- Updates velocity based on keyboard inputs for movement.
- Handles special actions like shooting and power-up activation.

### Key Features:
1. **Movement Handling:**
   - Processes directional inputs (`KEY_UP`, `KEY_DOWN`, `KEY_LEFT`, `KEY_RIGHT`) to control entity movement.

2. **Shooting:**
   - Handles the `KEY_SPACE` input to spawn projectiles at the player's position.

3. **Power-Up Activation:**
   - Activates power-ups when the `KEY_E` input is pressed (if available).

### Collaboration:
This file interacts with:
- **Registry:** To access and modify components like `Velocity`, `Position`, and `InputComponent`.
- **GameNetwork:** To send movement and shooting updates to the server.
- **AudioManager:** To play sound effects for actions like shooting.

### Example Usage:
```cpp
InputSystem inputSystem(registry, network, audioManager, powerUpActive, powerUpTimer);
float deltaTime = GetFrameTime();
inputSystem.update(deltaTime);
```

### Key Variables:
- `powerUpActive`: Indicates if a power-up is currently active.
- `powerUpTimer`: Tracks the remaining time for the active power-up.
- `shootingCooldowns`: Tracks cooldown periods for firing projectiles.

### Design Considerations:
- **Real-Time Responsiveness:**
  - Ensures minimal latency between player input and game action.
- **Multiplayer Integration:**
  - Synchronizes player actions with the server via `GameNetwork`.
- **Modularity:**
  - Easily extendable for new input actions or features.

### Summary:
The `InputSystem` bridges player inputs with in-game actions, ensuring responsive and intuitive gameplay. Its integration with ECS components and the networking system makes it a vital part of the R-Type game.
