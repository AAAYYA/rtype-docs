# Network Overview

## Purpose
The network component of the R-Type project is designed to enable multiplayer functionality through a combination of TCP and UDP protocols. It facilitates communication between the client and the server, allowing for real-time interactions such as player movements, shooting, and game state synchronization.

---

## Components

### **1. Server**
The server manages all game rooms, players, and network messages. It ensures that game states are synchronized across all connected clients.

#### **Key Features:**
- Room management (creation, deletion, and joining).
- Broadcasting player movements and actions.
- Handling enemy spawning and shield pickups.
- Communicating with clients via UDP for real-time updates.
- Handling player disconnections.

#### **Main Functions:**
- **`run(asio::io_context &io_context)`**: Starts the server and processes asynchronous operations.
- **`start_receive()`**: Listens for incoming UDP messages from clients.
- **`createRoom(uint32_t numberMaxPlayer, std::string name)`**: Creates a new game room.
- **`deleteRoom(uint32_t roomId)`**: Deletes an existing room.
- **`joinRoom(uint32_t roomId, std::shared_ptr<Client> newClient)`**: Adds a player to a specified room.
- **`broadcastPlayerMovement(uint32_t playerId, float posX, float posY, uint32_t roomId)`**: Broadcasts player position updates.
- **`spawnEnemy(uint32_t roomId)`**: Spawns enemies in a specific room.
- **`broadcastEnnemieMovement()`**: Broadcasts enemy positions to all players.
- **`spawnShield(uint32_t roomId)`**: Spawns shields in the game room.
- **`broadcastShieldPickup(uint32_t shieldId, uint32_t roomId)`**: Notifies all players when a shield is picked up.

#### **Data Structures:**
- **`Room`**: Represents a game room, containing players, enemies, shields, and room settings.
- **`Client`**: Represents a player, including ID, username, and network endpoint.
- **`Enemy` & `Shield`**: Represent game objects with position and ID.

---

### **2. Client**
The client communicates with the server to send actions (e.g., movement, shooting) and receive updates.

#### **Key Features:**
- TCP-based connection setup.
- UDP-based real-time data exchange.
- Sending connection requests with a username.
- Receiving game state updates (e.g., enemy movements, player actions).

#### **Main Functions:**
- **`connectToServer()`**: Establishes a TCP connection with the server.
- **`sendConnectRequest(const std::string &username)`**: Sends a connection request containing the player's username.
- **`receiveResponse()`**: Processes responses from the server.

---

### **3. Protocols**

#### **TCP:**
- Used for reliable, ordered communication.
- Handles connection setup and teardown.
- Transfers large messages, such as connection requests and game initialization.

#### **UDP:**
- Used for real-time communication (e.g., player movements, shooting).
- Offers low latency at the cost of reliability.

---

## Communication Flow

### 1. **Connection Initialization:**
1. The client establishes a TCP connection to the server.
2. The client sends a connection request with its username.
3. The server responds with confirmation and game state initialization data.

### 2. **Gameplay Communication:**
- **Player Actions:**
  - Players send movement and shooting messages via UDP.
  - The server processes and broadcasts these actions to other players in the same room.

- **Enemy and Shield Updates:**
  - The server spawns enemies and shields periodically.
  - Updates are broadcasted to all players in the room.

### 3. **Disconnection:**
- The server detects disconnection events and removes the player from the corresponding room.

---

## Design Considerations

1. **Real-Time Responsiveness:**
   - UDP is used for time-sensitive updates to minimize latency.

2. **Reliability:**
   - TCP ensures reliable delivery of critical messages (e.g., connection setup).

3. **Scalability:**
   - Rooms allow multiple games to run independently.
   - Each room handles its own set of players and game objects.

4. **Error Handling:**
   - Robust error handling ensures the server continues running despite individual failures (e.g., player disconnections).

---

## Example Usage

### **Client Code:**
```cpp
#include "Client.hpp"

int main() {
    std::string serverAddress = "127.0.0.1";
    int serverPort = 4242;
    rtype::Client client(serverAddress, serverPort);

    client.connectToServer();
    client.sendConnectRequest("Player1");
    client.receiveResponse();

    return 0;
}
```

### **Server Initialization:**
```cpp
asio::io_context io_context;
rtype::Server server(io_context, "127.0.0.1", 4343, "My R-Type Server");
server.run(io_context);
