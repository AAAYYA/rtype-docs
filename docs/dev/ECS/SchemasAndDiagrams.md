# Schemas and Diagrams

This section provides visual representations of the ECS architecture and its components to facilitate a deeper understanding of their relationships and workflows.

---

## High-Level ECS Architecture

### Diagram
```plaintext
[Entity]
    |
    +---> [Components]
    |
    +---> [Systems]
          |
          +---> [Registry]
                |
                +---> [SparseArray<Component>]
```

### Explanation
- **Entity:** A unique identifier that links to components.
- **Components:** Data structures attached to entities.
- **Systems:** Operate on entities with specific components.
- **Registry:** Manages components using `SparseArray`.

---

## Component Management Workflow

### Diagram
```plaintext
[Registry]
    |
    +---> [SparseArray<Position>]
    |          +---> [Optional Components]
    |
    +---> [SparseArray<Velocity>]
               +---> [Optional Components]
```

### Explanation
1. **Registry:** Maintains `SparseArray` instances for each component type.
2. **SparseArray:** Provides efficient storage and retrieval for components.
3. **Optional Components:** Ensure memory is only allocated when necessary.

---

## System Interaction

### Diagram
```plaintext
[SystemManager]
    |
    +---> [SystemBase]
    |          +---> [Specific Systems]
    |
    +---> [RenderingSystemBase]
               +---> [Rendering Systems]
```

### Explanation
- **SystemManager:** Oversees system execution and prioritization.
- **SystemBase:** Base class for all systems.
- **RenderingSystemBase:** Specialized base class for rendering systems.
- **Specific Systems:** Examples include MovementSystem, TimerSystem, and PhysicsSystem.

---

## Event Flow

### Diagram
```plaintext
[System/Handler] ---> [EventManager]
        |                    |
        +--- Generates        +--- Dispatches
             Events                 Events
                                 |
                            [Processing Systems]
```

### Explanation
1. **Event Generation:** Systems or input handlers create events.
2. **Event Dispatch:** The `EventManager` queues and distributes events.
3. **Event Processing:** Relevant systems handle dispatched events.

---

## File References
- [Overview](./Overview.md): Introduction to ECS.
- [Core Components](./CoreComponents.md): Details on `SparseArray` and `Registry`.
- [Systems](./Systems.md): Explanation of system functionality.
- [System Manager](./SystemManager.md): Details on orchestrating system execution.
- [Event Management](./EventManagement.md): Integration of events into ECS workflows.

---

## Next Steps
Refer to [API Reference](./APIReference.md) for detailed documentation of classes and methods.

