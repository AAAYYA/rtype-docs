# Core Components

This section details the core building blocks of the ECS architecture, specifically focusing on `SparseArray` and `Registry`.

---

## SparseArray
The `SparseArray` is a core data structure that provides efficient storage and retrieval of components.

### Overview
The `SparseArray` class stores components in an optional manner, allowing:
- Dynamic resizing.
- Efficient indexing and retrieval.
- Memory optimization by only allocating space when necessary.

### Key Features
- **Dynamic Growth:** Automatically resizes when accessing out-of-bounds indices.
- **Optional Storage:** Components are wrapped in `std::optional`, ensuring memory is only allocated when needed.
- **Type Safety:** Templated to ensure components are strongly typed.

### Implementation Details
```cpp
template <typename Component>
class SparseArray {
public:
    using value_type = std::optional<Component>;
    using reference_type = value_type&;

    reference_type operator[](size_t idx);
    reference_type insert_at(size_t pos, const Component& component);
    void erase(size_t pos);
};
```

### Methods
- **`operator[](size_t idx)`**
  - Access or create a component at the specified index.
- **`insert_at(size_t pos, const Component&)`**
  - Inserts a component at the given position, resizing if necessary.
- **`erase(size_t pos)`**
  - Removes the component at the specified position.

---

## Registry
The `Registry` serves as the central manager for entities and their components.

### Overview
The `Registry` is responsible for:
- Creating and destroying entities.
- Managing components associated with entities.
- Providing access to component arrays for systems to operate on.

### Key Features
- **Entity Management:** Each entity is represented by a unique identifier.
- **Component Association:** Maps entities to components using `SparseArray`.
- **System Compatibility:** Facilitates interaction between systems and components.

### Implementation Details
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
  - Returns the `SparseArray` for the specified component type.
- **`add_component<Component>(size_t entity, Component component)`**
  - Attaches a component to the given entity.
- **`remove_component<Component>(size_t entity)`**
  - Detaches the component from the specified entity.

---

## Relationships
### Diagram
```plaintext
[Registry]
    |
    +---> [SparseArray<Component>]
    |          +---> [Components]
    +---> [SparseArray<Component>]
               +---> [Components]
```

The `Registry` maintains multiple `SparseArray` instances, one for each component type.

---

## File References
- [Systems](./Systems.md): Explore how systems use components managed by `SparseArray` and `Registry`.
- [API Reference](./APIReference.md): Detailed documentation of `SparseArray` and `Registry` methods.

---

## Next Steps
Proceed to [Systems](./Systems.md) to understand how logic is applied to entities using the components managed by `SparseArray` and `Registry`.

