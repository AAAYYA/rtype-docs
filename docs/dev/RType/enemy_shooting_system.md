## EnemyShootingSystem.hpp

### Purpose:
The `EnemyShootingSystem` class is responsible for managing enemy projectiles, including firing behavior, timing, and integration with the ECS architecture.

### Includes:
```cpp
#include "EnemyShootingSystem.hpp"
#include "Registry.hpp"
#include "Component.hpp"
#include <iostream>
```

### File Description:
This file defines the `EnemyShootingSystem`, which handles periodic shooting actions for enemies in the game. The system ensures that enemies spawn projectiles at regular intervals, enhancing gameplay challenge.

### Key Classes and Functions:

#### **Class: EnemyShootingSystem**

**Constructor:**
```cpp
EnemyShootingSystem(Registry &registry);
```
- Initializes the system with a reference to the ECS registry, which manages game entities and their components.

**Functions:**

1. **update(float delta_time):**
   - Updates the state of the system every frame.
   - Tracks shooting intervals and triggers projectile creation when the timer elapses.

2. **shoot_projectiles():**
   - Iterates over enemy entities and spawns projectiles at their positions.
   - Assigns appropriate components (e.g., `Position`, `Velocity`, `EntityTypeComponent`, and `ProjectileTypeComponent`) to the new projectiles.

### Collaboration:
This file interacts with:
- **Registry:** To spawn new projectiles and manage components.
- **Components:** Uses `Position`, `Velocity`, `EntityTypeComponent`, and `ProjectileTypeComponent` to define projectile behavior.

### Key Variables:
- `shoot_timer`: Tracks time elapsed since the last projectile spawn.
- `shoot_interval`: Determines how frequently enemies fire projectiles.

### Example Usage:
```cpp
EnemyShootingSystem enemyShootingSystem(registry);
while (gameRunning) {
    float deltaTime = GetFrameTime();
    enemyShootingSystem.update(deltaTime);
}
```

### Design Considerations:
- **Scalability:**
  - The system supports multiple enemies firing independently.
  - Can be extended to include different projectile types or firing patterns.
- **Performance:**
  - Efficiently updates all enemies using ECS, minimizing redundant calculations.

### Summary:
The `EnemyShootingSystem` is a critical gameplay system, ensuring dynamic enemy interactions through projectile attacks. It maintains a balance between challenge and performance, integrating seamlessly with other game systems.
