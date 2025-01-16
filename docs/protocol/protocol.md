# R-Type Multiplayer Protocol Specification
# Version: 1.0
# Last Updated: 2024-11-26

#################################################
# 1. GENERAL INFORMATION
#################################################
PROTOCOL: TCP
PORT: 12345
MESSAGE_FORMAT: JSON
ENCODING: UTF-8

#################################################
# 2. MESSAGE TYPES AND FIELDS
#################################################

### 2.1. Connection Messages

# CLIENT → SERVER
MESSAGE: connect
FIELDS:
  - username: STRING (Player's chosen username)
EXAMPLE:
{
  "type": "connect",
  "username": "PlayerName"
}

# SERVER → CLIENT
MESSAGE: connected
FIELDS:
  - playerId: INTEGER (Unique player ID assigned by the server)
  - otherPlayers: ARRAY (List of connected players with their IDs and positions)
EXAMPLE:
{
  "type": "connected",
  "playerId": 1,
  "otherPlayers": [
    {
      "playerId": 2,
      "username": "OtherPlayer",
      "position": { "x": 100, "y": 200 }
    }
  ]
}

---

### 2.2. Movement Messages

# CLIENT → SERVER
MESSAGE: move
FIELDS:
  - playerId: INTEGER (Player's ID)
  - position: OBJECT (New coordinates of the player)
EXAMPLE:
{
  "type": "move",
  "playerId": 1,
  "position": { "x": 150, "y": 300 }
}

# SERVER → ALL CLIENTS
MESSAGE: update_position
FIELDS:
  - playerId: INTEGER (Player's ID)
  - position: OBJECT (Updated coordinates of the player)
EXAMPLE:
{
  "type": "update_position",
  "playerId": 1,
  "position": { "x": 150, "y": 300 }
}

---

### 2.3. Shooting Messages

# CLIENT → SERVER
MESSAGE: shoot
FIELDS:
  - playerId: INTEGER (Player's ID)
  - position: OBJECT (Coordinates where the bullet is fired)
  - direction: STRING (Direction of the shot, e.g., "left", "right")
EXAMPLE:
{
  "type": "shoot",
  "playerId": 1,
  "position": { "x": 200, "y": 320 },
  "direction": "right"
}

# SERVER → ALL CLIENTS
MESSAGE: bullet
FIELDS:
  - playerId: INTEGER (Player's ID who fired)
  - position: OBJECT (Coordinates where the bullet is)
  - direction: STRING (Direction of the shot, e.g., "left", "right")
  - source: STRING (Indicates the source of the bullet: "self", "ally", or "enemy")
EXAMPLE:
{
  "type": "bullet",
  "playerId": 1,
  "position": { "x": 200, "y": 320 },
  "direction": "right",
  "source": "self"
}


---

### 2.4. Enemy Spawn Messages

# SERVER → ALL CLIENTS
MESSAGE: spawn_enemy
FIELDS:
  - enemyId: INTEGER (Unique ID of the enemy)
  - position: OBJECT (Spawn coordinates of the enemy)
EXAMPLE:
{
  "type": "spawn_enemy",
  "enemyId": 101,
  "position": { "x": 800, "y": 250 }
}

---

### 2.5. Enemy Update Messages

# SERVER → ALL CLIENTS
MESSAGE: update_enemy
FIELDS:
  - enemyId: INTEGER (Unique ID of the enemy)
  - position: OBJECT (Updated coordinates of the enemy)
EXAMPLE:
{
  "type": "update_enemy",
  "enemyId": 101,
  "position": { "x": 750, "y": 250 }
}

---

### 2.6. Score Update Messages

# SERVER → ALL CLIENTS
MESSAGE: update_score
FIELDS:
  - playerId: INTEGER (ID of the player)
  - score: INTEGER (Player's current score)
EXAMPLE:
{
  "type": "update_score",
  "playerId": 1,
  "score": 5000
}

---

### 2.7. Disconnection Messages

# CLIENT → SERVER
MESSAGE: disconnect
FIELDS:
  - playerId: INTEGER (ID of the disconnecting player)
EXAMPLE:
{
  "type": "disconnect",
  "playerId": 1
}

# SERVER → ALL CLIENTS
MESSAGE: player_disconnected
FIELDS:
  - playerId: INTEGER (ID of the player who disconnected)
EXAMPLE:
{
  "type": "player_disconnected",
  "playerId": 1
}

---

#################################################
# 3. GAMEPLAY CYCLE
#################################################

1. **Connection Initialization**:
   - The client sends a `connect` message with a chosen username.
   - The server responds with `connected` containing the player ID and the list of other connected players.

2. **Gameplay Interaction**:
   - The client sends `move` and `shoot` messages to the server.
   - The server broadcasts `update_position`, `bullet`, `spawn_enemy`, and `update_enemy` messages to all clients.

3. **Score Management**:
   - The server periodically sends `update_score` to all clients.

4. **Disconnection**:
   - The client sends a `disconnect` message.
   - The server notifies other clients using `player_disconnected`.

#################################################
# END OF PROTOCOL
#################################################
