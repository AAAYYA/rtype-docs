# ECS-Based R-Type Architecture Documentation

## Overview
This section describes the implementation of a shooter game (RType) using the Entity Component System (ECS) framework. The game combines core ECS principles with systems such as movement, input, shooting, and rendering. Additional features include network communication and enemy behavior.

---

## Components and Systems

### 1. Shooting System
**File:** `ShootingSystem.hpp`
- A system designed to handle shooting logic for entities.
- Currently, it is a placeholder and does not contain active logic.

#### Key Implementation:
```cpp
class ShootingSystem : public SystemBase {
public:
    ShootingSystem(Registry& registry) : _registry(registry) {}

    void update(float delta_time) override {
    }

private:
    Registry& _registry;
};
```

---

### 2. Game Networking
**File:** `GameNetwork.hpp`
- Manages communication with the game server.
- Provides methods for sending and receiving messages, such as player connection, movement, and disconnection.

#### Key Methods:
1. **`GameNetwork` Constructor:**
   - Establishes a TCP connection with the server.
   - Throws an error if the connection fails.

2. **`sendConnectMessage`:**
   - Sends a message to connect the player to the server with a username.

3. **`receivePlayerID`:**
   - Receives the player ID from the server after connection.

4. **`sendMoveMessage`:**
   - Sends the player's position updates to the server.

---

### 3. Game Setup and Initialization
**File:** `GameSetup.hpp`
- Configures the ECS by registering components and adding systems to the `SystemManager`.
- Spawns the player entity and initializes its components.

#### Key Implementation:
1. **`initializeECS`:**
   - Registers all required components (e.g., `Position`, `Velocity`).
   - Adds systems such as `InputSystem`, `MovementSystem`, and `CollisionSystem` to the `SystemManager`.

2. **`playGame`:**
   - Spawns a player entity and initializes its components.
   - Enters the main game loop, calling system updates and rendering each frame.
   - Ends the game if the player's health drops to 0 or the window is closed.

#### Example Player Initialization:
```cpp
ECS::Entity player = registry.spawn_entity();
registry.get_components<ECS::Position>().insert_at(static_cast<size_t>(player), ECS::Position{100.0f, 300.0f});
registry.get_components<ECS::Velocity>().insert_at(static_cast<size_t>(player), ECS::Velocity{0.0f, 0.0f});
registry.get_components<ECS::Drawable>().insert_at(static_cast<size_t>(player), ECS::Drawable{});
registry.get_components<ECS::InputComponent>().insert_at(static_cast<size_t>(player), ECS::InputComponent{});
registry.get_components<ECS::Health>().insert_at(static_cast<size_t>(player), ECS::Health{5});
```

---

## Game Flow

### ECS Initialization
1. Register all required components with the `Registry`.
2. Add systems to the `SystemManager`, specifying their priority if necessary.
3. Spawn the initial player entity and configure its components.

### Main Game Loop
1. **Update Systems:**
   - Systems such as `InputSystem`, `MovementSystem`, and `CollisionSystem` are updated every frame based on the elapsed time (`delta_time`).

2. **Render Frame:**
   - Clears the screen and begins drawing.
   - Calls the rendering system to display all drawable entities.

3. **Health Check:**
   - If the player's health reaches zero, the game transitions to the menu state.

4. **End Game:**
   - The game ends when the player exits the window or their health depletes.

---

## Summary
This implementation demonstrates the integration of ECS principles into a functioning game. Key features include:
- Modular design with systems for input, movement, shooting, and collision.
- Efficient management of entities and components through the `Registry`.
- Networking capabilities for player communication.

By separating logic into distinct systems and leveraging the ECS architecture, the game achieves a scalable and maintainable design.

