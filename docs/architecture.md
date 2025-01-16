# R-Type Game Documentation

## Table of Contents
1. Introduction
2. Project Structure
3. Components
4. Systems
5. Managers
6. Network
7. Utilities
8. Main Files

## Introduction
R-Type is a multiplayer shoot 'em up game where players control a spaceship and fight against waves of enemies. This documentation provides an overview of the project's structure, components, systems, managers, network communication, and utilities.

## Project Structure
The project is organized into several directories, each containing specific types of files:

- 

src

: Contains the main source code files.
  - `ECS/`: Entity-Component-System architecture implementation.
    - `include/`: Header files for ECS components and systems.
    - 

src

: Source files for ECS components and systems.
  - `Modules/`: Contains modules for audio and graphics.
    - `Audio/`: Audio management.
      - `include/`: Header files for audio management.
      - 

src

: Source files for audio management.
    - `Graphics/`: Graphics management.
      - `include/`: Header files for graphics management.
      - 

src

: Source files for graphics management.
  - `Network/`: Network communication implementation.
  - `RType/`: Game-specific logic and systems.
    - `include/`: Header files for game logic and systems.
    - 

src

: Source files for game logic and systems.
  - `Tests/`: Unit tests for the project.
  - `Utils/`: Utility files.

## Components
Components are data structures that hold the state of entities. They are stored in sparse arrays within the 

Registry

.

### Position
```cpp
struct Position {
    float x, y;
    Position() = default;
    Position(float posX, float posY) : x(posX), y(posY) {}
};
```

### Velocity
```cpp
struct Velocity {
    float vx = 0.0f;
    float vy = 0.0f;
    Velocity() = default;
    Velocity(float vx, float vy) : vx(vx), vy(vy) {}
};
```

### Drawable
```cpp
struct Drawable {};
```

### InputComponent
```cpp
struct InputComponent {
    bool right = false;
    bool left = false;
    bool up = false;
    bool down = false;
};
```

### Projectile
```cpp
struct Projectile {
    bool is_player_projectile;
    Projectile(bool is_player_projectile) : is_player_projectile(is_player_projectile) {}
};
```

### Enemy
```cpp
struct Enemy {
    float speed;
    Enemy(float speed) : speed(speed) {}
};
```

### ColorComponent
```cpp
struct ColorComponent {
    float r, g, b, a;
    ColorComponent() = default;
    ColorComponent(float r, float g, float b, float a) : r(r), g(g), b(b), a(a) {}
};
```

### Hitbox
```cpp
struct Hitbox {
    float width;
    float height;
    Hitbox(float width = 1.0f, float height = 1.0f) : width(width), height(height) {}
};
```

### EntityType
```cpp
enum class EntityType {
    PLAYER,
    PROJECTILE,
    ENEMY
};
```

### EntityTypeComponent
```cpp
struct EntityTypeComponent {
    EntityType type;
    EntityTypeComponent(EntityType type) : type(type) {}
};
```

## Systems
Systems are responsible for updating entities based on their components.

### MovementSystem
```cpp
class MovementSystem : public SystemBase {
public:
    MovementSystem(Registry& registry) : _registry(registry) {}
    void update(float delta_time) override;
private:
    Registry& _registry;
};
```

### InputSystem
```cpp
class InputSystem : public SystemBase {
public:
    InputSystem(Registry &registry, GameNetwork &network, AudioManager &audioManager);
    void update(float delta_time) override;
private:
    Registry &_registry;
    GameNetwork &_network;
    AudioManager &_audioManager;
};
```

### ShootingSystem
```cpp
class ShootingSystem : public SystemBase {
public:
    ShootingSystem(Registry& registry) : _registry(registry) {}
    void update(float delta_time) override;
private:
    Registry& _registry;
};
```

### EnemySpawnSystem
```cpp
class EnemySpawnSystem : public SystemBase {
public:
    EnemySpawnSystem(Registry& registry);
    void update(float delta_time) override;
    void spawn_enemy_from_server(uint32_t enemyId, float x, float y);
private:
    Registry& _registry;
    float spawn_timer;
    float spawn_interval;
    float enemy_speed;
};
```

### EnemyShootingSystem
```cpp
class EnemyShootingSystem : public SystemBase {
public:
    EnemyShootingSystem(Registry &registry);
    void update(float delta_time) override;
private:
    Registry &_registry;
    float shoot_timer;
    float shoot_interval;
    void shoot_projectiles();
};
```

### CollisionSystem
```cpp
class CollisionSystem : public SystemBase {
public:
    CollisionSystem(Registry &registry, AudioManager &audioManager);
    void update(float delta_time) override;
private:
    Registry &_registry;
    AudioManager _audioManager;
    bool check_collision(const Position &posA, const Hitbox &boxA, const Position &posB, const Hitbox &boxB);
    void handle_collision(size_t entityA, size_t entityB);
    void handle_player_hit(size_t player_index);
};
```

## Managers
Managers handle specific aspects of the game, such as audio and graphics.

### AudioManager
```cpp
class AudioManager {
public:
    AudioManager();
    static Sound& loadSound(const std::string& id, const std::string& filepath);
    static Sound& getSound(const std::string& id);
    static void playSound(const std::string& id);
    static bool isSoundLoaded(const std::string& id);
    ~AudioManager();
private:
    static std::unordered_map<std::string, Sound> sounds;
};
```

### GraphicsManager
```cpp
class GraphicsManager {
public:
    GraphicsManager();
    static Texture2D& loadTexture(const std::string& id, const std::string& filepath);
    static Texture2D& getTexture(const std::string& id);
    static void loadGifAsSpriteSheet(const std::string& id, const std::string& filepath, int rows, int cols);
    static Texture2D& getGifFrame(const std::string& id, int frameIndex);
    static bool isTextureLoaded(const std::string& id);
    static bool isGifLoaded(const std::string& id);
    ~GraphicsManager();
private:
    static std::unordered_map<std::string, Texture2D> textures;
    static std::unordered_map<std::string, std::vector<Texture2D>> gifs;
};
```

## Network
The network module handles communication between the client and server.

### GameNetwork
```cpp
class GameNetwork {
public:
    GameNetwork(asio::io_context& io_context, const std::string& serverIP, int serverPort, int udpPort, ECS::Registry &registry);
    ~GameNetwork();
    void sendConnectMessage(const std::string& username);
    uint32_t receivePlayerID();
    void sendDisconnectMessage();
    void sendMoveMessage(float posX, float posY);
    void start_send();
    void start_receive();
    std::function<void(uint32_t enemyId, float x, float y)> notifyEnemySpawn;
    void handleSpawnEnemyMessage(const char* buffer);
private:
    int tcpSocket;
    int udpServPort;
    sockaddr_in serverAddr;
    asio::ip::udp::socket udpSocket;
    asio::ip::udp::endpoint server_endpoint_;
    std::string message_;
    enum { max_length = 1024 };
    char data_[max_length];
    ECS::Registry& registry;
    std::unordered_map<uint32_t, ECS::Entity> playerIdMap;
public:
    uint32_t playerId;
};
```

### Server
```cpp
class Server {
public:
    Server(asio::io_context& io_context, const std::string& serverIP, int udpPort);
    ~Server();
    void start_receive();
    void run(asio::io_context& io_context);
    void handleClient(int clientSocket);
    void handleMessage(uint8_t messageType, const char *buffer, size_t bytesRead, int socket, const sockaddr_in *udpClientAddr, asio::ip::udp::endpoint serverEndpoint);
    void handleUDPMessages();
    void broadcastBinaryMessage(uint8_t messageType, uint32_t playerId, float posX, float posY, uint8_t state);
    void handleConnect(int clientSocket, const char *buffer);
    void handleDisconnect(uint32_t playerId);
    void handleMoveMessage(const char* buffer);
    void handleShootMessage(const char *buffer);
    void sendBinaryMessage(int clientSocket, uint8_t messageType, const void *data, size_t dataSize);
    void spawnEnemy();
    void enemySpawnLoop();
private:
    int playerCounter;
    int enemyCounter;
    int udpServPort;
    asio::ip::udp::socket udpSocket;
    std::unordered_map<uint32_t, Player> players;
    std::unordered_map<uint32_t, asio::ip::udp::endpoint> senderEndpoints;
    bool stopServer;
};
```

### Client
```cpp
class Client {
public:
    Client(const std::string &serverAddress, int serverPort);
    ~Client();
    void connectToServer();
    void sendConnectRequest(const std::string &username);
    void receiveResponse();
private:
    std::string serverAddress;
    int serverPort;
    int tcpSocket;
};
```

## Utilities
Utility files provide additional functionality used throughout the project.

### Timer
```cpp
#ifndef TIMER_HPP_
#define TIMER_HPP_
#endif /* !TIMER_HPP_ */
```

### Logger
```cpp
#ifndef LOGGER_HPP_
#define LOGGER_HPP_
#endif /* !LOGGER_HPP_ */
```

### Config
```cpp
#ifndef CONFIG_HPP_
#define CONFIG_HPP_
#endif /* !CONFIG_HPP_ */
```

## Main Files
The main files initialize and run the game.

### Main Client
```cpp
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

### Main Server
```cpp
int main() {
    try {
        std::signal(SIGINT, signalHandler); 
        std::signal(SIGTERM, signalHandler);
        asio::io_context io_context;
        rtype::Server server(io_context, "127.0.0.1", 4343);
        std::cout << "[Rtype SERVER]: starting the server" << std::endl;
        std::thread serverThread([&server, &io_context]() {
            try {
                server.run(io_context);
            } catch (const std::exception &ex) {
                std::cerr << "[Rtype SERVER]: server error ..." << ex.what() << std::endl;
                running = false;
            }
        });
        while (running) {
            std::this_thread::sleep_for(std::chrono::seconds(1));
        }
        std::cout << "[Rtype SERVER]: Shutting down ..." << std::endl;
        if (serverThread.joinable()) {
            serverThread.detach();
        }
        std::cout << "[Rtype SERVER]: Server stopped gracefully" << std::endl;
        return EXIT_SUCCESS;
    } catch(const rtype::Exception &ex) {
        std::cerr << "[Rtype SERVER]: Exception found" << ex.what() << std::endl;
        return EXIT_FAILURE;
    } catch (const std::exception &ex) {
        std::cerr << "Fatal error: " << ex.what() << std::endl;
        return EXIT_FAILURE;
    } catch (...) {
        std::cerr << "Unknown fatal error" << std::endl;
        return EXIT_FAILURE;
    }
}
```

### Main Game
```cpp
int main() {
    const int screenWidth = 800;
    const int screenHeight = 600;
    InitWindow(screenWidth, screenHeight, "R-Type Standalone");
    InitAudioDevice();

    AudioManager audioManager;
    audioManager.loadSound("shoot", "../assets/sounds/laser.mp3");
    audioManager.loadSound("game_over", "../assets/sounds/game_over.mp3");
    audioManager.loadSound("explosion", "../assets/sounds/explosion.mp3");

    GraphicsManager graphicsManager;
    graphicsManager.loadTexture("player", "../assets/sprites/player1.png");
    graphicsManager.loadGifAsSpriteSheet("laser", "../assets/sprites/laser.gif", 1, 2);
    graphicsManager.loadGifAsSpriteSheet("enemyLaser", "../assets/sprites/enemyLaser.gif", 1, 2);
    graphicsManager.loadGifAsSpriteSheet("power", "../assets/sprites/power.gif", 1, 4);
    graphicsManager.loadGifAsSpriteSheet("enemy1", "../assets/sprites/enemy1.gif", 1, 3);

    GameState state = GameState::MENU;
    ECS::Registry registry;
    ECS::SystemManager systemManager;
    asio::io_context io_context;
    GameNetwork network(io_context, "127.0.0.1", 4242, 4343, registry);

    std::string username = "Player1";
    network.sendConnectMessage(username);
    network.playerId = network.receivePlayerID();
    std::cout << "[Game] Connected with Player ID: " << network.playerId << "\n";

    network.notifyEnemySpawn = [&](uint32_t enemyId, float x, float y) {
        GameSetup::spawnEnemy(registry, x, y);
    };

    GameSetup::initializeECS(registry, systemManager, network, audioManager, graphicsManager);
    std::thread ioThread([&io_context]() {
        io_context.run();
    });

    while (state != GameState::EXIT && !WindowShouldClose()) {
        if (state == GameState::MENU) {
            show_menu(state);
        } else if (state == GameState::SETTINGS) {
            show_settings(state);
        } else if (state == GameState::PLAYING) {
            systemManager.update_all(GetFrameTime());
            BeginDrawing();
            ClearBackground(RAYWHITE);
            systemManager.render_all();
            EndDrawing();
        }
    }

    CloseAudioDevice();
    CloseWindow();
    return 0;
}
