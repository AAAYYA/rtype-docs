# Entity Component System (ECS) Overview

## What is ECS?
The Entity Component System (ECS) is an architectural pattern used primarily in game development to achieve modularity, scalability, and performance. It decouples data (components) from behavior (systems) and uses entities to represent objects in the game world.

### Core Concepts
1. **Entities**: Unique identifiers (usually integers) representing objects in the game world.
2. **Components**: Data containers holding the attributes of entities, such as position, velocity, or health.
3. **Systems**: Logic processors that operate on entities with specific components.

This ECS implementation leverages these principles to allow flexible management of game objects and their behaviors.

---

## How It Works

### 1. **Entities**
Entities are simply IDs that act as placeholders for components. They do not store data or behavior directly.

Example:
- Entity 1: May represent a player.
- Entity 2: May represent an enemy.

Entities gain functionality by associating them with components.

### 2. **Components**
Components are plain data structures that hold attributes. Each type of component (e.g., `Position`, `Velocity`) is stored separately in the system, allowing efficient data access and manipulation.

Example Components:
- `Position`: Contains `x` and `y` coordinates.
- `Velocity`: Contains `vx` and `vy` for movement speed.

### 3. **Systems**
Systems are responsible for processing entities that have the required components. For example, a `movement_system` updates the positions of all entities that have `Position` and `Velocity` components.

#### Key Features of Systems:
- Operate only on specific components.
- Contain logic (e.g., movement, rendering).
- Decoupled from other systems, enhancing modularity.

Example:
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

---

## Purpose of ECS
The ECS pattern separates data from behavior, allowing for:
- **Modularity**: Systems are independent and can be added or removed without affecting others.
- **Performance**: Grouping components by type improves memory access patterns.
- **Scalability**: The system can manage thousands of entities efficiently.

---

## How to Use This ECS

### 1. Creating Components
Define components as plain data structures.
```cpp
struct Position {
    float x, y;
};

struct Velocity {
    float vx, vy;
};
```

### 2. Adding Components to Entities
Use the `Registry` to associate components with entities.
```cpp
Registry registry;
Entity player = registry.create_entity();
registry.add_component(player, Position{0, 0});
registry.add_component(player, Velocity{1, 1});
```

### 3. Defining Systems
Create a system by inheriting from `SystemBase` or implementing a standalone function.
```cpp
class MovementSystem : public SystemBase {
public:
    void update(float delta_time) override {
    }
};
```

### 4. Managing Systems
Add systems to the `SystemManager` and execute them in the game loop.
```cpp
SystemManager manager;
manager.add_system(std::make_shared<MovementSystem>());

while (game_running) {
    manager.update_all(delta_time);
}
```

---

## Summary
The ECS framework provides:
- A clear separation of data, behavior, and entities.
- A modular approach to adding and managing systems.
- Scalability for complex games with thousands of entities.

By following these principles, developers can achieve a maintainable and efficient architecture for their game projects.

