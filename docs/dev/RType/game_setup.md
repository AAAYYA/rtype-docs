## GameSetup.hpp

### Purpose:
The `GameSetup` class initializes the ECS (Entity Component System), registers components, and sets up systems necessary for the R-Type game. It also handles the creation of player and enemy entities.

### Includes:
```cpp
#include <raylib.h>
#include <iostream>
#include "GameSetup.hpp"
#include "Component.hpp"
#include "MovementSystem.hpp"
#include "InputSystem.hpp"
#include "ShootingSystem.hpp"
#include "EnemySpawnSystem.hpp"
#include "EnemyShootingSystem.hpp"
#include "CollisionSystem.hpp"
#include "GameCommon.hpp"
#include "RaylibRenderingSystem.hpp"
#include "Health.hpp"
```

### File Description:
This file contains the `GameSetup` class, which encapsulates the initialization process for the game. It ensures all ECS components and systems are properly registered and that essential entities like the player are created.

### Key Functions:

#### **Function: initializeECS**
```cpp
void initializeECS(ECS::Registry &registry, ECS::SystemManager &systemManager, GameNetwork &network, AudioManager &audioManager, GraphicsManager &graphicsManager, bool &powerUpActive, float &powerUpTimer, float &powerUpProgress);
```
- Registers all required components and systems with the ECS.
- Systems added include movement, input handling, shooting, enemy spawning, and collision detection.

#### **Function: spawnEnemy**
```cpp
void spawnEnemy(ECS::Registry &registry, float x, float y);
```
- Spawns an enemy entity at the specified position.
- Assigns components like `Position`, `Velocity`, `Drawable`, `EntityTypeComponent`, and `Health`.

#### **Function: playGame**
```cpp
void playGame(ECS::Registry &registry, ECS::SystemManager &systemManager, GameState &state, GameNetwork &network, AudioManager &audioManager, GraphicsManager &graphicsManager, bool &powerUpActive, float &powerUpTimer, float &powerUpProgress);
```
- Handles the main gameplay loop.
- Updates systems, checks player state, and renders the game frame by frame.

### Collaboration:
This file interacts with:
- **ECS:** Registers components and manages systems.
- **GameNetwork:** For player movement and entity synchronization.
- **GraphicsManager:** For rendering sprites and animations.
- **AudioManager:** To play sounds like shooting and explosions.

### Key Variables:
- `powerUpActive`: Tracks whether a power-up is currently active.
- `powerUpTimer`: Times the duration of an active power-up.
- `powerUpProgress`: Tracks progress toward the next power-up activation.

### Example Usage:
```cpp
GameSetup gameSetup;
gameSetup.initializeECS(registry, systemManager, network, audioManager, graphicsManager, powerUpActive, powerUpTimer, powerUpProgress);
gameSetup.playGame(registry, systemManager, gameState, network, audioManager, graphicsManager, powerUpActive, powerUpTimer, powerUpProgress);
```

### Design Considerations:
- **Modularity:**
  - Each system and component is initialized independently for flexibility.
- **Scalability:**
  - New systems and components can be added without modifying existing code.
- **Performance:**
  - Ensures efficient initialization to minimize startup time.

### Summary:
`GameSetup.hpp` orchestrates the initialization and management of all essential systems and components for the R-Type game. It provides the foundation for a smooth and scalable gameplay experience.
