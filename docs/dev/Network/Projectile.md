## Projectile.md

### Purpose
The `Projectile` system is responsible for defining the behavior, types, and attributes of projectiles in the game. Projectiles are used for various in-game mechanics, such as player and enemy attacks.

### Includes
```cpp
#include "ProjectileType.hpp"
#include "Position.hpp"
#include "Velocity.hpp"
#include "Collision.hpp"
#include "GameCommon.hpp"
```

### File Description
This file contains the `Projectile` system, which defines the core properties and behaviors of projectiles within the game. It handles initialization, updates, and interactions of projectile entities in the ECS framework.

### Key Components and Systems

#### **ProjectileType**
Defines the type of the projectile, including:
- Damage value.
- Speed.
- Direction.
- Special properties (e.g., explosive, piercing).

#### **Position**
Tracks the projectile's position in the game world.

#### **Velocity**
Determines the projectile's movement by applying velocity vectors to its position.

#### **Collision**
Detects interactions between the projectile and other entities (e.g., players, enemies, walls).

### Functions and Behavior

#### **Function: createProjectile**
```cpp
void createProjectile(ECS::Registry &registry, float x, float y, float speed, Direction direction, ProjectileType type);
```
- Spawns a new projectile entity.
- Assigns necessary components, such as `Position`, `Velocity`, `ProjectileType`, and `Collision`.
- Configures the projectile's speed and direction based on input parameters.

#### **Function: updateProjectiles**
```cpp
void updateProjectiles(ECS::Registry &registry, float deltaTime);
```
- Updates projectile positions based on their velocity and direction.
- Handles interactions, such as collision detection and damage application.
- Removes projectiles that leave the game bounds or collide with targets.

### Collaboration
The `Projectile` system interacts with the following:
- **MovementSystem**: To update projectile positions over time.
- **CollisionSystem**: To manage interactions with other entities.
- **HealthSystem**: To apply damage to entities hit by projectiles.

### Design Considerations

- **Efficiency**: Optimized for handling a large number of projectiles simultaneously without impacting performance.
- **Modularity**: Supports easy addition of new projectile types and behaviors.
- **Scalability**: Designed to handle varying speeds, trajectories, and effects for different projectile types.

### Example Usage
```cpp
createProjectile(registry, playerX, playerY, 300.0f, Direction::UP, ProjectileType::PLAYER_BULLET);
updateProjectiles(registry, deltaTime);
```

### Summary
The `Projectile` system is essential for implementing dynamic and interactive gameplay. By managing projectile entities and their interactions, it enables mechanics such as player attacks, enemy projectiles, and environmental effects.
