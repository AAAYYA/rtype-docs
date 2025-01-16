## Entity.md

### Purpose:
The `Entity` class serves as the base class for game entities in the R-Type project. It defines the common interface and properties that all entities (such as players, enemies, or other in-game objects) must implement.

### File Location:
`src/Network/Entity.hpp`

### Key Responsibilities:
- Provide a common interface for accessing and modifying entity positions.
- Serve as a parent class for specific entity implementations, such as `Player`.

### Class Definition:

#### **Class: `Entity`**
```cpp
class Entity {
public:
    virtual ~Entity() = 0;
    virtual float getXposition() = 0;
    virtual float getYposition() = 0;
    virtual float setXposition(float x) = 0;
    virtual float setYposition(float y) = 0;

protected:
    float _x;
    float _y;
};
```

#### Key Methods:

##### Destructor:
```cpp
virtual ~Entity() = 0;
```
- Defined as a pure virtual destructor to make `Entity` an abstract class.
- Ensures that all derived classes provide their own implementation.

##### Getters and Setters:
```cpp
virtual float getXposition() = 0;
virtual float getYposition() = 0;
virtual float setXposition(float x) = 0;
virtual float setYposition(float y) = 0;
```
- **Purpose:**
  - `getXposition` and `getYposition` retrieve the entity's position.
  - `setXposition` and `setYposition` modify the entity's position.
- **Implementation:** Must be defined in derived classes, such as `Player`.

### Usage:
The `Entity` class is not directly instantiated but serves as a base class for entities that require position management.

Example:
```cpp
class Player : public Entity {
public:
    float getXposition() override {
        return _x;
    }

    float getYposition() override {
        return _y;
    }

    float setXposition(float x) override {
        _x = x;
        return _x;
    }

    float setYposition(float y) override {
        _y = y;
        return _y;
    }
};
```

### Collaboration:
- **Player:** Inherits from `Entity` to define specific behaviors for player entities.
- **Server:** May use `Entity`-derived objects to manage positions of game elements.

### Design Considerations:
- **Abstraction:**
  - The `Entity` class ensures that all derived entities share a consistent interface for position management.
- **Flexibility:**
  - By making the class abstract, it allows other entities to implement their own logic while adhering to the interface.

### Summary:
`Entity` is a foundational class that provides a consistent interface for managing position-related data in the R-Type project. Its abstract design ensures flexibility and scalability for various entity types.
