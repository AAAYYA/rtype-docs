# Systems

Systems in the ECS architecture define the logic that operates on entities with specific components. They encapsulate functionality, making the architecture modular and scalable.

---

## Key Systems

### 1. Movement System
The Movement System updates the positions of entities based on their velocity components.

#### Purpose
To apply velocity to position, effectively moving entities within the game world.

#### Implementation
```cpp
void movement_system(Registry& registry) {
    auto& positions = registry.get_components<Position>();
    auto& velocities = registry.get_components<Velocity>();

    for (size_t i = 0; i < positions.size() && i < velocities.size(); ++i) {
        if (positions[i] && velocities[i]) {
            positions[i]->x += velocities[i]->vx;
            positions[i]->y += velocities[i]->vy;
        }
    }
}
```

#### Explanation
- Iterates through entities with `Position` and `Velocity` components.
- Updates the `x` and `y` attributes of the position using the velocity values.

---

### 2. Timer System
The Timer System tracks durations and executes actions when timers expire.

#### Purpose
To manage timed events for entities, such as animations or cooldowns.

#### Implementation
```cpp
class TimerSystem {
public:
    void update(ECS::Registry& registry, float deltaTime) {
        auto& timers = registry.get_components<Timer>();

        for (size_t i = 0; i < timers.size(); ++i) {
            if (!timers[i]) continue;

            auto& timer = *timers[i];
            timer.elapsed += deltaTime;

            if (timer.elapsed >= timer.duration) {
                timer.elapsed = 0.0f;
                if (!timer.repeat) {
                    registry.remove_component<Timer>(i);
                }
            }
        }
    }
};
```

#### Explanation
- Updates each timer’s elapsed time.
- Resets or removes the timer when it expires, based on its `repeat` flag.

---

### 3. Physics System
The Physics System handles interactions like collisions between entities.

#### Purpose
To process and resolve physical interactions.

#### Implementation (Example Placeholder)
```cpp
void physics_system(Registry& registry) {
    // Example collision resolution logic here.
}
```

#### Explanation
- Identifies entities with collidable components.
- Resolves overlaps, applying forces or constraints as needed.

---

## Relationship with Core Components
Systems leverage the `Registry` to access components and iterate over entities. For example:

- The `Movement System` accesses `Position` and `Velocity` components.
- The `Timer System` interacts with `Timer` components.
- The `Physics System` may combine `Position`, `Velocity`, and `Collidable` components.

### Diagram
```plaintext
[System]
    |
    +---> [Registry]
    |          +---> [SparseArray<Component>]
    +---> [Entities with Components]
```

---

## File References
- [Core Components](./CoreComponents.md): Details on `Registry` and `SparseArray`.
- [System Manager](./SystemManager.md): Managing and executing multiple systems.
- [Schemas and Diagrams](./SchemasAndDiagrams.md): Visual flow of how systems interact with components and entities.

---

## Next Steps
Proceed to [Event Management](./EventManagement.md) to explore how events integrate with systems for decoupled logic and dynamic interactions.

