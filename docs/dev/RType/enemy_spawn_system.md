## EnemySpawnSystem.hpp

### Purpose:
The `EnemySpawnSystem` class is responsible for spawning enemy entities at regular intervals or based on server events, ensuring a consistent and challenging gameplay experience.

### Includes:
```cpp
#include "EnemySpawnSystem.hpp"
#include "Registry.hpp"
#include "Component.hpp"
#include <iostream>
```

### File Description:
This file defines the `EnemySpawnSystem`, which handles the creation and initialization of enemy entities. It ensures enemies are added to the game world at appropriate times and positions, with configurable properties like speed and spawn intervals.

### Key Classes and Functions:

#### **Class: EnemySpawnSystem**

**Constructor:**
```cpp
EnemySpawnSystem(Registry &registry);
```
- Initializes the system with a reference to the ECS registry.

**Functions:**

1. **update(float delta_time):**
   - Updates the state of the system every frame.
   - Tracks the spawn timer and spawns enemies when the interval elapses.
   - Handles cleanup of off-screen enemies.

2. **spawn_enemy_from_server(uint32_t enemyId, float x, float y):**
   - Spawns an enemy at a specific position, triggered by server events.

### Collaboration:
This file interacts with:
- **Registry:** To create new enemy entities and manage components.
- **GameNetwork:** For receiving server instructions to spawn enemies.
- **Components:** Uses `Position`, `Velocity`, `Drawable`, `EntityTypeComponent`, and `Health` to define enemy attributes.

### Key Variables:
- `spawn_timer`: Tracks time elapsed since the last enemy spawn.
- `spawn_interval`: Determines how frequently enemies spawn.
- `enemy_speed`: Specifies the default speed of spawned enemies.

### Example Usage:
```cpp
EnemySpawnSystem enemySpawnSystem(registry);
while (gameRunning) {
    float deltaTime = GetFrameTime();
    enemySpawnSystem.update(deltaTime);
}
```

### Design Considerations:
- **Dynamic Difficulty:**
  - The spawn rate and enemy attributes can be adjusted to scale difficulty dynamically.
- **Server Integration:**
  - Allows real-time updates to the spawn logic based on server events, ensuring synchronized multiplayer gameplay.
- **Performance:**
  - Optimized to handle large numbers of enemies without degrading performance.

### Summary:
The `EnemySpawnSystem` is essential for maintaining a steady flow of challenges in the game. Its integration with the server ensures synchronized enemy spawning across multiplayer sessions, while its modular design allows for easy customization and scalability.
