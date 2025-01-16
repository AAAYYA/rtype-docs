## HealthSystem.hpp

### Purpose:
The `HealthSystem` class manages the health state of game entities, ensuring that health-related events such as entity destruction or shield depletion are handled appropriately.

### Includes:
```cpp
#include "HealthSystem.hpp"
#include "Registry.hpp"
#include "Component.hpp"
```

### File Description:
This file defines the `HealthSystem`, which is responsible for monitoring and updating the health of entities in the game. The system also ensures that entities are removed or updated when their health reaches zero.

### Key Functions:

#### **Function: update**
```cpp
void update(float delta_time);
```
- Iterates through all entities with a `Health` component.
- Checks if any entity’s health has dropped to zero or below.
- Handles the removal of entities or other logic when health is depleted.

### Collaboration:
This file interacts with:
- **Registry:** To access and modify the `Health` components of entities.
- **Components:**
  - `Health`: Tracks the current and maximum health of entities.
  - Other related components (e.g., `ShieldComponent`) can be integrated if needed.

### Example Usage:
```cpp
HealthSystem healthSystem(registry);
float deltaTime = GetFrameTime();
healthSystem.update(deltaTime);
```

### Design Considerations:
- **Efficiency:**
  - Uses ECS to efficiently process only entities with a `Health` component.
- **Flexibility:**
  - Can be extended to include health regeneration or effects triggered at specific health thresholds.

### Summary:
The `HealthSystem` plays a critical role in ensuring the proper handling of entity health within the game. Its efficient processing and modular design make it a cornerstone of the ECS-based architecture, allowing for future expansions such as healing effects or additional health-based mechanics.
