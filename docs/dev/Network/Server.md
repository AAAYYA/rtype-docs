## Server.md

### Purpose:
The `Server` class orchestrates the server-side logic for the R-Type game. It manages player connections, rooms, and in-game events, ensuring a smooth multiplayer experience. The server is responsible for:
- Handling UDP/TCP communication with clients.
- Managing rooms, players, enemies, and shields.
- Broadcasting events such as player movements, enemy spawns, and shield pickups.

### Includes:
```cpp
#include <iostream>
#include <thread>
#include <vector>
#include <unordered_map>
#include <mutex>
#include <string>
#include <memory>
#include <netinet/in.h>
#include <asio.hpp>
#include "raylib.h"

#include "Player.hpp"
#include "IServer.hpp"
```

### Key Structures:

#### **Room**
Represents a game room where players can interact with each other.
```cpp
struct Room {
    std::unordered_map<uint32_t, std::shared_ptr<Client>> players;
    std::unordered_map<uint32_t, Enemy> enemies;
    std::unordered_map<uint32_t, Shield> shields;
    uint32_t shieldCounter = 0;
    uint32_t roomId;
    bool isGameStarted;
    uint32_t numberMaxPlayer;
    uint32_t playerCounter;
    uint32_t enemyCounter;
    std::string roomName;
};
```

#### **Server**
Handles all server-side logic, including room management and message handling.
```cpp
class Server {
private:
    uint32_t id;
    std::string servName;
    asio::ip::udp::socket udpSocket;
    asio::ip::udp::endpoint serverEndpoint;
    std::mutex playersMutex;
    std::mutex enemiesMutex;
    std::mutex roomsMutex;

    bool stopServer;
    enum { max_length = 1024 };
    char data_[max_length];

    std::unordered_map<uint32_t, std::shared_ptr<Room>> rooms;

public:
    int udpServPort;
    Server(asio::io_context &io_context, const std::string &serverIP, int udpPort, std::string name);
    ~Server();

    void run(asio::io_context &io_context);
    void start_receive();
    void createRoom(uint32_t numberMaxPlayer, std::string name);
    bool deleteRoom(uint32_t roomId);
    void joinRoom(uint32_t roomId, std::shared_ptr<Client> newClient);
    void leaveRoom(uint32_t roomId, uint32_t playerId);
    std::shared_ptr<Room> getRoom(uint32_t roomId);
    void kickPlayer(uint32_t playerId, uint32_t roomId);

private:
    void handleMessage(uint8_t messageType, const char *buffer, size_t bytesRead, asio::ip::udp::endpoint serverEndpoint);
    void handleDisconnect(uint32_t playerId);
    void handleMoveMessage(const char *buffer);
    void handleShootMessage(const char *buffer);
    void broadcastPlayerMovement(uint32_t playerId, float posX, float posY, uint32_t roomId);
    void broadcastEnnemieMovement();
    void spawnEnemy(uint32_t roomId);
    void enemySpawnLoop();
    void spawnShield(uint32_t roomId);
    void broadcastShieldPickup(uint32_t shieldId, uint32_t roomId);
};
```

### Key Methods:

#### **Room Management**
- **createRoom**: Initializes a new room with a unique ID and specified parameters.
- **deleteRoom**: Removes a room from the server.
- **joinRoom**: Adds a player to a room, creating one if necessary.
- **kickPlayer**: Removes a player from a room.

#### **Message Handling**
- **handleMessage**: Processes incoming messages from clients.
- **handleMoveMessage**: Updates player movement and broadcasts the position.
- **handleShootMessage**: Handles shooting events and notifies other players in the room.

#### **Gameplay Events**
- **spawnEnemy**: Spawns an enemy in a room and notifies players.
- **enemySpawnLoop**: Periodically spawns enemies in active rooms.
- **spawnShield**: Generates a shield power-up and broadcasts it.
- **broadcastShieldPickup**: Notifies players of a shield pickup.

### Collaboration:
- **Client**: Receives updates about gameplay, movement, and events from the server.
- **Room**: Holds the state of players, enemies, and game objects.
- **ASIO**: Manages asynchronous network operations for UDP communication.

### Example Usage:
```cpp
asio::io_context io_context;
Server server(io_context, "127.0.0.1", 4343, "R-Type Server");
server.run(io_context);
```

### Design Considerations:
- **Concurrency**: Uses mutexes to ensure thread-safe access to shared resources.
- **Error Handling**: Catches exceptions during network operations to maintain stability.
- **Scalability**: Supports multiple rooms with dynamic player and object management.

### Summary:
The `Server` class is a central component of the R-Type game's multiplayer functionality. It provides robust room management, player coordination, and in-game event handling, ensuring a seamless and engaging experience for all participants.
