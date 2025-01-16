# Player.md

## Purpose:
The `Player` class represents individual players in the R-Type game. Each player has unique attributes like ID, username, position, and an associated UDP endpoint for network communication.

## Key Features:
- Tracks player-specific information, such as their position and username.
- Provides an interface to modify and access player attributes.
- Encapsulates networking-related details like the UDP endpoint.

---

## Header File: `Player.hpp`

### Key Attributes:
```cpp
private:
    uint32_t _id;                // Unique player ID
    std::string _username;       // Player's username
    struct sockaddr_in _udpEndpoint; // Player's network endpoint
    float _x;                    // X-coordinate position
    float _y;                    // Y-coordinate position
```

### Key Methods:
```cpp
public:
    Player(uint32_t playerId);   // Constructor initializes player ID
    ~Player();                   // Destructor

    uint32_t getID() const;      // Retrieves player ID
    void setID(uint32_t id);     // Sets player ID

    std::string getUsername() const;  // Retrieves player's username
    void setUsername(const std::string &username); // Sets player's username

    struct sockaddr_in& getUDPEndpoint(); // Accesses player's UDP endpoint
    void setUDPEndpoint(const struct sockaddr_in &endpoint); // Sets UDP endpoint

    float getXposition() override; // Retrieves X-coordinate
    float getYposition() override; // Retrieves Y-coordinate
    float setXposition(float x) override; // Updates X-coordinate
    float setYposition(float y) override; // Updates Y-coordinate
```

---

## Implementation File: `Player.cpp`

### Constructor and Destructor:
- **`Player::Player(uint32_t playerId)`**: Initializes a player with the specified ID and default position `(0.0f, 0.0f)`.
- **`Player::~Player()`**: Handles cleanup when a player object is destroyed.

### Attribute Management:
- **ID Management:**
  ```cpp
  uint32_t Player::getID() const {
      return this->_id;
  }

  void Player::setID(uint32_t id) {
      this->_id = id;
  }
  ```

- **Username Management:**
  ```cpp
  std::string Player::getUsername() const {
      return this->_username;
  }

  void Player::setUsername(const std::string &username) {
      this->_username = username;
  }
  ```

- **Position Management:**
  ```cpp
  float Player::getXposition() {
      return this->_x;
  }

  float Player::getYposition() {
      return this->_y;
  }

  float Player::setXposition(float x) {
      this->_x = x;
      return this->_x;
  }

  float Player::setYposition(float y) {
      this->_y = y;
      return this->_y;
  }
  ```

- **UDP Endpoint Management:**
  ```cpp
  struct sockaddr_in& Player::getUDPEndpoint() {
      return this->_udpEndpoint;
  }

  void Player::setUDPEndpoint(const struct sockaddr_in &endpoint) {
      this->_udpEndpoint = endpoint;
  }
  ```

---

## Design Considerations:
- **Encapsulation:**
  - The class provides a clean interface to manage player-specific data while encapsulating implementation details.

- **Integration with Network Module:**
  - The `Player` class works closely with the `Server` class for managing player communication over UDP.

- **Scalability:**
  - Supports expansion to include more player-related features such as inventory, health, or score tracking.

---

## Collaboration:
- **Server Class:**
  - The `Player` objects are managed by the `Server` class, which uses their attributes for communication and game state updates.

---

## Example Usage:
```cpp
#include "Player.hpp"

rtype::Player player(1); // Create a player with ID 1
player.setUsername("PlayerOne");
player.setXposition(100.0f);
player.setYposition(200.0f);

std::cout << "Player " << player.getUsername() << " is at ("
          << player.getXposition() << ", " << player.getYposition() << ")\n";
```

---

## Summary:
The `Player` class is a fundamental component of the R-Type game's network and gameplay systems. It ensures efficient and organized management of player-specific data and integrates seamlessly with other modules.
