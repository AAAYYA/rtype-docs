# System Manager and Rendering

The System Manager orchestrates the execution of multiple systems, ensuring that they run in the correct order and priority. Rendering systems are specialized systems responsible for drawing entities and the game world.

---

## System Manager

### Overview
The `SystemManager` class organizes and manages the lifecycle of systems. It:
- Supports dynamic addition and removal of systems.
- Prioritizes systems based on their assigned priority values.
- Updates and renders systems as required.

### Implementation
```cpp
class SystemManager {
public:
    void add_system(std::shared_ptr<SystemBase> system, int priority = 0);
    void update_all(float delta_time);
    void render_all();

private:
    std::vector<std::pair<int, std::shared_ptr<SystemBase>>> _systems;
};
```

### Methods
- **`add_system(std::shared_ptr<SystemBase> system, int priority)`**
  - Adds a system to the manager with a priority value.
  - Systems with lower priority values are executed first.

- **`update_all(float delta_time)`**
  - Calls the `update` method of each system in priority order.

- **`render_all()`**
  - Calls the `render` method of rendering systems, ensuring they draw in the correct sequence.

---

### Workflow
1. Systems are registered with the `SystemManager` using `add_system`.
2. During each game loop iteration:
   - **Update Phase:** All systems are updated via `update_all`.
   - **Render Phase:** Rendering systems are invoked via `render_all`.

---

## Rendering System

### Overview
Rendering systems specialize in drawing entities and their components. They inherit from `RenderingSystemBase`, extending it with specific rendering logic.

### Base Class
```cpp
class RenderingSystemBase : public SystemBase {
public:
    virtual void render() = 0;
    virtual ~RenderingSystemBase() = default;
};
```

### Example: Simple Rendering System
```cpp
class SimpleRenderingSystem : public RenderingSystemBase {
public:
    void update(float delta_time) override {
        // Update logic, if needed
    }

    void render() override {
        // Rendering logic, e.g., draw entities
    }
};
```

---

## Relationship Diagram
```plaintext
[SystemManager]
    |
    +---> [SystemBase]
    |          +---> [Specific Systems]
    |
    +---> [RenderingSystemBase]
               +---> [Rendering Systems]
```

---

## Example Usage

### Registering Systems
```cpp
SystemManager manager;
manager.add_system(std::make_shared<MovementSystem>(), 1);
manager.add_system(std::make_shared<SimpleRenderingSystem>(), 0);
```

### Executing Systems
```cpp
manager.update_all(delta_time);
manager.render_all();
```

---

## File References
- [Systems](./Systems.md): General details about system functionality.
- [Event Management](./EventManagement.md): Events that can trigger rendering or updates.
- [Schemas and Diagrams](./SchemasAndDiagrams.md): Additional visualizations of system architecture.

---

## Next Steps
Proceed to [Schemas and Diagrams](./SchemasAndDiagrams.md) for visual representations of the ECS architecture and its components.

