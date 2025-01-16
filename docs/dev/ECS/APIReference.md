# API Reference

This section provides detailed documentation for the classes and methods used in the ECS architecture.

---

## SparseArray

### Class Definition
```cpp
template <typename Component>
class SparseArray {
public:
    using value_type = std::optional<Component>;
    using reference_type = value_type&;

    reference_type operator[](size_t idx);
    reference_type insert_at(size_t pos, const Component& component);
    void erase(size_t pos);
    size_t size() const;
};
```

### Methods
- **`operator[](size_t idx)`**
  - **Description:** Access or create a component at the specified index.
  - **Parameters:**
    - `size_t idx`: Index of the component.
  - **Returns:** Reference to the optional component.

- **`insert_at(size_t pos, const Component&)`**
  - **Description:** Insert a component at the specified position, resizing if necessary.
  - **Parameters:**
    - `size_t pos`: Position to insert the component.
    - `const Component&`: The component to insert.
  - **Returns:** Reference to the inserted component.

- **`erase(size_t pos)`**
  - **Description:** Removes the component at the specified position.
  - **Parameters:**
    - `size_t pos`: Position of the component to erase.

- **`size() const`**
  - **Description:** Returns the number of elements in the sparse array.
  - **Returns:** The size of the sparse array.

---

## Registry

### Class Definition
```cpp
class Registry {
public:
    template <typename Component>
    SparseArray<Component>& get_components();

    template <typename Component>
    void add_component(size_t entity, Component component);

    template <typename Component>
    void remove_component(size_t entity);
};
```

### Methods
- **`get_components<Component>()`**
  - **Description:** Retrieve the `SparseArray` for a specific component type.
  - **Returns:** Reference to the `SparseArray` of the specified component type.

- **`add_component<Component>(size_t entity, Component component)`**
  - **Description:** Attaches a component to the given entity.
  - **Parameters:**
    - `size_t entity`: The entity ID.
    - `Component component`: The component to attach.

- **`remove_component<Component>(size_t entity)`**
  - **Description:** Detaches a component from the specified entity.
  - **Parameters:**
    - `size_t entity`: The entity ID.

---

## SystemManager

### Class Definition
```cpp
class SystemManager {
public:
    void add_system(std::shared_ptr<SystemBase> system, int priority = 0);
    void update_all(float delta_time);
    void render_all();
};
```

### Methods
- **`add_system(std::shared_ptr<SystemBase> system, int priority)`**
  - **Description:** Adds a system to the manager with a specified priority.
  - **Parameters:**
    - `std::shared_ptr<SystemBase> system`: The system to add.
    - `int priority`: Priority value for execution order (lower executes first).

- **`update_all(float delta_time)`**
  - **Description:** Calls the `update` method of all registered systems in priority order.
  - **Parameters:**
    - `float delta_time`: Time elapsed since the last update.

- **`render_all()`**
  - **Description:** Invokes the `render` method of all rendering systems.

---

## EventManager

### Class Definition
```cpp
class EventManager {
public:
    void add_event(const Event& event);
    std::vector<Event> get_events(Event::EventType type);
    void clear_events();
};
```

### Methods
- **`add_event(const Event& event)`**
  - **Description:** Adds an event to the queue.
  - **Parameters:**
    - `const Event& event`: The event to add.

- **`get_events(Event::EventType type)`**
  - **Description:** Retrieves all events of a specific type.
  - **Parameters:**
    - `Event::EventType type`: The type of event to retrieve.
  - **Returns:** A vector of events of the specified type.

- **`clear_events()`**
  - **Description:** Clears all events from the queue.

---

## RenderingSystemBase

### Class Definition
```cpp
class RenderingSystemBase : public SystemBase {
public:
    virtual void render() = 0;
    virtual ~RenderingSystemBase() = default;
};
```

### Methods
- **`render()`**
  - **Description:** Pure virtual method to render entities and visuals.
  - **Returns:** None.

---

## File References
- [Overview](./Overview.md): Introduction to the ECS architecture.
- [Core Components](./CoreComponents.md): Building blocks like `SparseArray` and `Registry`.
- [Systems](./Systems.md): Functional systems operating on entities.
- [Event Management](./EventManagement.md): Decoupled event-driven logic.
- [System Manager](./SystemManager.md): Orchestrating and rendering systems.
- [Schemas and Diagrams](./SchemasAndDiagrams.md): Visual representations.

---

## Next Steps
For practical implementation examples, proceed to [Examples](./Examples.md).

