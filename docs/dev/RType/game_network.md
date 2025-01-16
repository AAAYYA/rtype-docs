## GameNetwork.hpp

### Purpose:
The `GameNetwork` class handles all networking-related functionalities for the R-Type game, enabling communication between the server and the client. This includes player actions, entity synchronization, and event handling.

### Includes:
```cpp
#include <arpa/inet.h>
#include <cstring>
#include <iostream>
#include <chrono>
#include <thread>
#include "GameNetwork.hpp"
#include "Registry.hpp"
```

### File Description:
This file defines the `GameNetwork` class, which provides asynchronous networking functionality using the ASIO library. It facilitates communication with the game server, sending and receiving messages for multiplayer synchronization.

### Key Classes and Functions:

#### **Class: GameNetwork**

**Constructor:**
```cpp
GameNetwork(asio::io_context &io_context, const std::string &serverIP, int udpPort, ECS::Registry &registry);
```
- Initializes the network with the specified server IP, port, and ECS registry.

**Destructor:**
```cpp
~GameNetwork();
```
- Cleans up any resources or connections during destruction.

**Functions:**

1. **start_send():**
   - Begins sending messages asynchronously to the server.

2. **start_receive():**
   - Listens for incoming messages from the server and processes them.

3. **sendConnectMessage(const std::string &username):**
   - Sends a connection request to the server with the player’s username.

4. **sendDisconnectMessage():**
   - Notifies the server that the player is disconnecting.

5. **sendMoveMessage(float posX, float posY):**
   - Sends the player’s movement updates to the server.

6. **sendShootMessage(uint32_t playerId, float posX, float posY, float dirX, float dirY):**
   - Sends a shoot action to the server, including projectile position and direction.

7. **requestServerInfo(asio::io_context &io_context, int port):**
   - Requests a list of available game servers from the server endpoint.

8. **handleSpawnEnemyMessage(const char *buffer):**
   - Processes server messages to spawn enemies at specific positions.

### Collaboration:
This file interacts with:
- **Registry:** To update entity states based on server messages.
- **ECS Components:** Manages synchronization of entities like players, enemies, and projectiles.
- **Server:** Communicates using UDP to send/receive game state updates.

### Key Variables:
- `server_endpoint_`: Stores the server’s address and port.
- `udpSocket`: Handles UDP communication.
- `playerIdMap`: Maps player IDs to ECS entities for synchronization.

### Example Usage:
```cpp
GameNetwork network(io_context, "127.0.0.1", 4343, registry);
network.sendConnectMessage("Player1");
network.start_receive();
```

### Design Considerations:
- **Asynchronous Communication:**
  - Uses ASIO for non-blocking network operations, ensuring smooth gameplay.
- **Multiplayer Support:**
  - Handles multiple players, synchronizing their states with minimal latency.
- **Error Handling:**
  - Includes mechanisms to retry failed operations or recover from network interruptions.

### Summary:
The `GameNetwork` class is the backbone of multiplayer functionality in the R-Type game. By enabling seamless communication with the server, it ensures synchronized gameplay and a responsive experience for all players.
