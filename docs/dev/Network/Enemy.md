## Enemy.md

### Purpose:
The `Enemy` structure and its related functionalities are essential for managing enemy entities in the R-Type game. This documentation provides details about its attributes and integration into the game's systems.

### Implementation Location:
`Server.hpp`

### Description:
The `Enemy` structure represents a single enemy entity within a game room. It encapsulates its attributes and behaviors.

### Attributes:

#### `id`
- **Type:** `uint32_t`
- **Description:** Unique identifier for the enemy.

#### `x`
- **Type:** `float`
- **Description:** X-coordinate position of the enemy on the game map.

#### `y`
- **Type:** `float`
- **Description:** Y-coordinate position of the enemy on the game map.

### Integration:
- **Room Management:** 
  Enemies are part of the `Room` structure and are managed within the `rooms` map in the `Server` class.
- **Spawn Logic:** 
  Enemies are spawned dynamically by calling the `spawnEnemy` function in the `Server` class.

### Key Functions:

#### **Function:** `spawnEnemy`
```cpp
void spawnEnemy(uint32_t roomId);
```
- **Purpose:**
  Spawns an enemy at a random position in the specified room.
- **Logic:**
  - Generates a random X and Y position for the enemy.
  - Assigns a unique ID to the enemy.
  - Adds the enemy to the `enemies` map in the corresponding `Room`.
  - Broadcasts the enemy spawn message to all players in the room.

#### **Function:** `broadcastEnnemieMovement`
```cpp
void broadcastEnnemieMovement();
```
- **Purpose:**
  Broadcasts enemy movement updates to all players in the respective rooms.
- **Logic:**
  - Iterates over all active rooms and their enemies.
  - Prepares and sends a movement update message for each enemy to the players.

### Communication:
- **UDP Messages:**
  Enemies' movements and spawns are communicated to players via UDP messages.

#### Example Spawn Broadcast:
Message Type: `0x09`
- **Payload:**
  - `enemyId`: Identifier of the enemy.
  - `x`: X-coordinate.
  - `y`: Y-coordinate.

### Example Integration:
#### Spawning an Enemy
```cpp
server.spawnEnemy(roomId);
```

#### Broadcasting Enemy Movement
```cpp
server.broadcastEnnemieMovement();
```

### Summary:
The `Enemy` structure and its associated logic provide a streamlined approach for managing enemies in the game. From spawning to broadcasting movements, the system ensures consistent and efficient gameplay mechanics.
