## Client.md

### Purpose:
The `Client` class manages client-side interactions for the R-Type game. It handles the connection to the server, sending messages, and receiving responses. This class encapsulates network communication for the client.

---

### Includes:
```cpp
#include <iostream>
#include <string>
#include <thread>
#include <memory>
#include <json/json.h>
#include <sys/socket.h>
#include <arpa/inet.h>
#include <unistd.h>
```

---

### File Description:
The `Client` class facilitates communication with the game server over TCP. It provides methods to establish a connection, send a connection request with the player's username, and process server responses.

---

### Key Functions:

#### **Constructor:**
```cpp
Client(const std::string &serverAddress, int serverPort);
```
- Initializes the client with the server's IP address and port.
- Sets up the TCP socket for communication.

#### **Destructor:**
```cpp
~Client();
```
- Closes the TCP socket upon destruction if it is open.

#### **Function: connectToServer**
```cpp
void connectToServer();
```
- Establishes a TCP connection to the server.
- Prints a success or failure message to the console.

#### **Function: sendConnectRequest**
```cpp
void sendConnectRequest(const std::string &username);
```
- Sends a JSON-formatted connection request containing the player's username to the server.
- Prints the sent username to the console.

#### **Function: receiveResponse**
```cpp
void receiveResponse();
```
- Listens for and processes responses from the server.
- Prints the received response or an error message.

---

### Example Usage:
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

---

### Design Considerations:
- **Modularity:**
  - Separate methods for connecting, sending requests, and receiving responses make the class easy to maintain and extend.
- **Error Handling:**
  - Errors during socket creation, connection, or communication are logged to the console for debugging.
- **Scalability:**
  - Additional methods can be added for other client-side functionalities like game events or chat messages.

---

### Collaboration:
- Communicates with the server (`Server` class).
- Utilizes JSON for message formatting and parsing.
- Works alongside `Player` and `Entity` components for game-related data.

---

### Summary:
The `Client` class is a critical component of the R-Type game's network module. It handles the essential tasks of connecting to the server, sending player details, and processing server responses, ensuring seamless client-server communication.
