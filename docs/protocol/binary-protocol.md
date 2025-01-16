# R-Type Multiplayer Protocol Specification

## Version: 2.0  
**Last Updated:** 2024-12-04  

---

## 1. General Information

- **Protocol:** TCP  
- **Port:** 4242  
- **Message Format:** Binary  
- **Encoding:** UTF-8  
- **Packet Header**:  
  - **Type:** 1 byte (indicates message type)  
  - **Payload Size:** 4 bytes (indicates the size of the payload)  

---

## 2. Message Types and Fields

| **Type** | **Message**          | **Description**                      |
|----------|----------------------|--------------------------------------|
| `0x01`   | `connect`             | Client requests connection to the server |
| `0x02`   | `connected`           | Server confirms connection and provides initial state |
| `0x03`   | `disconnect`          | Client requests disconnection        |
| `0x04`   | `player_disconnected` | Server notifies clients of a player’s disconnection |
| `0x05`   | `move`                | Client sends updated position        |
| `0x06`   | `update_position`     | Server broadcasts player position updates |
| `0x07`   | `shoot`               | Client sends a shooting action       |
| `0x08`   | `bullet`              | Server broadcasts bullet position and state |
| `0x09`   | `spawn_enemy`         | Server broadcasts enemy spawn        |
| `0x0A`   | `update_enemy`        | Server broadcasts enemy movement     |
| `0x0B`   | `update_score`        | Server updates players' scores       |
| `0x0C`   | `chat_message`        | Client sends a chat message          |
| `0x0D`   | `broadcast_chat`      | Server broadcasts a chat message     |
| `0x0E`   | `power_up`            | Server broadcasts a power-up spawn   |
| `0x0F`   | `collect_power_up`    | Client collects a power-up           |
| `0x10`   | `sync_state`          | Server synchronizes game state       |
| `0x11`   | `error`               | Server sends an error message        |

---

### 2.1. Connection Messages

#### **Client → Server: `connect`**

| **Field**   | **Type** | **Description**                   |
|-------------|----------|-----------------------------------|
| `username`  | STRING   | Player's username (16 bytes max)  |

**Binary Format**:  
| **Byte Offset** | **Size (bytes)** | **Field**  | **Description**             |
|-----------------|------------------|------------|-----------------------------|
| 0               | 1                | `type`     | Message type (`0x01`)       |
| 1               | 4                | `size`     | Payload size (e.g., 16)     |
| 5               | 16               | `username` | Player's username           |

---

#### **Server → Client: `connected`**

| **Field**        | **Type**  | **Description**                                |
|------------------|-----------|------------------------------------------------|
| `playerId`       | INTEGER   | Unique player ID assigned by the server        |
| `gameState`      | OBJECT    | Current game state                             |

---

### 2.2. Gameplay Messages

#### **Client → Server: `move`**

| **Field**   | **Type**  | **Description**                 |
|-------------|-----------|---------------------------------|
| `playerId`  | INTEGER   | Player's ID                     |
| `position`  | OBJECT    | Player's updated coordinates    |

---

#### **Server → All Clients: `update_position`**

| **Field**   | **Type**  | **Description**                 |
|-------------|-----------|---------------------------------|
| `playerId`  | INTEGER   | Player's ID                     |
| `position`  | OBJECT    | Updated coordinates             |

---

### 2.3. Shooting Messages

#### **Client → Server: `shoot`**

| **Field**    | **Type** | **Description**                |
|--------------|----------|--------------------------------|
| `playerId`   | INTEGER  | Player's ID                    |
| `position`   | OBJECT   | Coordinates of the shot        |
| `direction`  | STRING   | Direction of the shot          |

---

### 2.4. Chat Messages

#### **Client → Server: `chat_message`**

| **Field**   | **Type** | **Description**                 |
|-------------|----------|---------------------------------|
| `username`  | STRING   | Player's username               |
| `message`   | STRING   | Chat message content            |

**Binary Format**:  
| **Byte Offset** | **Size (bytes)** | **Field**  | **Description**             |
|-----------------|------------------|------------|-----------------------------|
| 0               | 1                | `type`     | Message type (`0x0C`)       |
| 1               | 4                | `size`     | Payload size                |
| 5               | 16               | `username` | Player's username           |
| 21              | N                | `message`  | Chat message content        |

---

### 2.5. Power-Up Messages

#### **Server → All Clients: `power_up`**

| **Field**   | **Type**  | **Description**                 |
|-------------|-----------|---------------------------------|
| `powerUpId` | INTEGER   | Unique power-up ID              |
| `position`  | OBJECT    | Spawn coordinates               |
| `type`      | STRING    | Type of power-up                |

---

### 2.6. Error Handling Messages

#### **Server → Client: `error`**

| **Field**   | **Type**  | **Description**                 |
|-------------|-----------|---------------------------------|
| `errorCode` | INTEGER   | Error code                      |
| `message`   | STRING    | Error message                   |

---

## 3. Game State Synchronization

The server periodically sends a **`sync_state`** message to all connected clients to ensure they are up-to-date with the game world.

| **Field**   | **Type**  | **Description**                 |
|-------------|-----------|---------------------------------|
| `players`   | ARRAY     | List of all players and their states |
| `enemies`   | ARRAY     | List of all enemies and their states |
| `powerUps`  | ARRAY     | List of active power-ups        |

---

## 4. Error Codes

| **Error Code** | **Description**                 |
|----------------|---------------------------------|
| `1001`         | Invalid message format          |
| `1002`         | Unknown message type            |
| `1003`         | Player not found                |
| `1004`         | Action not permitted            |

---

## 5. Example Communication Sequence

1. **Client connects**:  
   - Sends `connect` message with username.  
   - Server responds with `connected` and player ID.

2. **Client moves**:  
   - Sends `move` message with new position.  
   - Server broadcasts `update_position`.

3. **Client shoots**:  
   - Sends `shoot` message with position and direction.  
   - Server broadcasts `bullet`.

4. **Player sends chat**:  
   - Sends `chat_message`.  
   - Server broadcasts `broadcast_chat`.

5. **Game synchronization**:  
   - Server periodically sends `sync_state` to all clients.

---

## 6. Packet Structure Overview

| **Header (Bytes)** | **Field**  | **Description**             |
|--------------------|------------|-----------------------------|
| 1                  | `type`     | Message type                |
| 4                  | `size`     | Payload size                |
| N                  | `payload`  | Actual message data         |

---

**End of Protocol**
