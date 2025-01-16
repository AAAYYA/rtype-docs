# Examples and Enhancements

This section provides practical examples of ECS usage and suggests potential enhancements to improve functionality and scalability.

---

## Examples

### 1. Creating an Entity with Components
This example demonstrates how to create an entity and attach components to it.

```cpp
#include "Registry.hpp"
#include "Component.hpp"

int main() {
    ECS::Registry registry;

    // Create an entity
    size_t player = registry.create_entity();

    // Add components
    registry.add_component<Position>(player, {0.0f, 0.0f});
    registry.add_component<Velocity>(player, {5.0f, 0.0f});

    return 0;
}
```

---

### 2. Updating Systems
Here’s how to update all systems within the `SystemManager`.

```cpp
#include "SystemManager.hpp"
#include "MovementSystem.hpp"

int main() {
    ECS::SystemManager manager;
    ECS::Registry registry;

    // Add a movement system
    manager.add_system(std::make_shared<ECS::MovementSystem>());

    // Game loop
    while (true) {
        float delta_time = 0.016f; // Example delta time (60 FPS)
        manager.update_all(delta_time);
    }

    return 0;
}
```

---

### 3. Event Handling
Example of generating and processing collision events.

```cpp
#include "EventManager.hpp"
#include "Event.hpp"

int main() {
    ECS::EventManager event_manager;

    // Generate a collision event
    event_manager.add_event(ECS::Event(ECS::Event::COLLISION, 1, 2));

    // Retrieve and process events
    auto collision_events = event_manager.get_events(ECS::Event::COLLISION);
    for (const auto& event : collision_events) {
        std::cout << "Collision detected between entities "
                  << event.entity_id << " and " << event.other_entity_id << std::endl;
    }

    return 0;
}
```

---

## Suggested Enhancements

### 1. Advanced Event System
- **Enhancement:** Introduce event listeners and subscriptions.
- **Benefits:** Enables more granular event handling and system notifications.

### 2. Threaded Systems
- **Enhancement:** Enable systems to run on separate threads for performance gains.
- **Challenges:** Requires careful synchronization of shared resources.

### 3. Debugging Tools
- **Enhancement:** Add visualization tools for entities, components, and systems.
- **Benefits:** Simplifies debugging and improves developer experience.

### 4. Expanded Component Types
- **Enhancement:** Include components for animations, sound, and AI behaviors.
- **Benefits:** Broadens the range of supported game mechanics.

---

## File References
- [Overview](./Overview.md): Introduction to the ECS architecture.
- [Core Components](./CoreComponents.md): Details on `SparseArray` and `Registry`.
- [Systems](./Systems.md): Functional systems operating on entities.
- [API Reference](./APIReference.md): Detailed documentation of ECS methods.
- [Schemas and Diagrams](./SchemasAndDiagrams.md): Visual representations.

---

## Next Steps
This concludes the ECS documentation. You are now equipped to develop, extend, and maintain an efficient ECS-based architecture.

