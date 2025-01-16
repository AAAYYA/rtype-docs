## CollisionSystem.hpp

### Purpose:
Defines the system responsible for detecting and handling collisions between entities in the game world.

### Includes:
```cpp
#include <iostream>
#include "CollisionSystem.hpp"
#include "Registry.hpp"
#include "AudioManager.hpp"
#include "Component.hpp"
#include "GameCommon.hpp"
```

### File Description:
This file defines the `CollisionSystem` class, which is part of the ECS architecture. The system is responsible for detecting and managing collisions between game entities such as players, enemies, projectiles, and power-ups.

### Key Classes and Functions:

#### **Class: CollisionSystem**

**Constructor:**
```cpp
CollisionSystem(Registry &registry, AudioManager &audioManager, float &powerUpProgress, bool &powerUpActive, float &powerUpTimer);
```
- Initializes the collision system with references to the ECS registry, audio manager, and power-up parameters.

**Functions:**

1. **update(float delta_time):**
   - Iterates through game entities to check for collisions and handle them accordingly.

2. **check_collision(const Position &posA, const Hitbox &boxA, const Position &posB, const Hitbox &boxB):**
   - Determines if two entities' hitboxes overlap.

3. **handle_collision(size_t entityA, size_t entityB):**
   - Resolves specific collision events based on entity types.

4. **remove_entity_components(size_t entity_index):**
   - Removes all components associated with an entity.

5. **handle_shield_pickup(size_t player_index, size_t shield_index):**
   - Handles the event when a player picks up a shield.

6. **handle_projectile_enemy_collision(size_t projectile_index, size_t enemy_index):**
   - Manages collisions between projectiles and enemies, updating health and score as necessary.

7. **handle_player_hit(size_t player_index):**
   - Handles when the player is hit, applying shield or health deductions as required.

### Collaboration:
This file interacts with the following components and files:
- **Registry:** For managing entities and components.
- **AudioManager:** For playing sound effects during collisions.
- **Components:** Utilizes `Position`, `Hitbox`, `EntityTypeComponent`, `Health`, and `ShieldComponent`.
- **GameCommon:** Shared utility functions such as drawing health bars and power-up progress bars.

### Example Usage:
```cpp
CollisionSystem collisionSystem(registry, audioManager, powerUpProgress, powerUpActive, powerUpTimer);
collisionSystem.update(deltaTime);
