## ProjectileType.hpp

### Purpose:
Defines the `ProjectileType` enumeration and associated components to classify and manage different types of projectiles in the game. This ensures flexible handling of projectiles for both players and enemies.

### Includes:
```cpp
#include "ProjectileType.hpp"
#include "Component.hpp"
```

### File Description:
This file provides an enumeration for projectile types and a component to attach projectile-specific data to entities. It facilitates distinguishing between player and enemy projectiles and customizing their behavior.

### Key Enumerations:

#### **Enum: ProjectileType**
```cpp
enum class ProjectileType {
    PLAYER_PROJECTILE,
    ENEMY_PROJECTILE
};
```
- **PLAYER_PROJECTILE:** Represents projectiles fired by the player.
- **ENEMY_PROJECTILE:** Represents projectiles fired by enemies.

### Key Components:

#### **Component: ProjectileTypeComponent**
```cpp
struct ProjectileTypeComponent {
    ProjectileType type;
};
```
- Encapsulates the type of projectile associated with an entity.
- Enables systems to identify and differentiate projectile behavior based on type.

### Collaboration:
This file interacts with:
- **Systems:**
  - **CollisionSystem:** To handle interactions between projectiles and other entities.
  - **EnemyShootingSystem:** To spawn enemy projectiles.
  - **InputSystem:** To spawn player projectiles.
- **Registry:** For associating the `ProjectileTypeComponent` with entities.

### Example Usage:
```cpp
ECS::Entity projectile = registry.spawn_entity();
registry.get_components<ECS::ProjectileTypeComponent>().insert_at(
    static_cast<size_t>(projectile),
    ECS::ProjectileTypeComponent{ProjectileType::PLAYER_PROJECTILE}
);
```

### Design Considerations:
- **Extensibility:**
  - Additional projectile types can be added, such as `POWER_UP_PROJECTILE` or `SPECIAL_ATTACK_PROJECTILE`.
- **Efficiency:**
  - The simple enumeration allows for quick identification and branching logic in systems.
- **Customizability:**
  - Systems can define unique behaviors for each projectile type (e.g., speed, damage, visuals).

### Summary:
`ProjectileType.hpp` is a foundational file for managing projectile classification and behavior in the R-Type game. Its simplicity and flexibility make it a critical component for gameplay mechanics involving shooting and projectile interactions.
