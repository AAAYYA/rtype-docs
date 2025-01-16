## IServer.md

### Purpose:
`IServer` defines the interface for the server implementation in the Network module of the R-Type project. It provides an abstraction layer for the server's functionality and serves as a blueprint for concrete server implementations.

### Location:
**Path:** `Network/IServer.hpp`

### Key Features:
- **Interface Design:** Provides a uniform structure for the server to implement essential functionalities.
- **Integration:** Ensures compatibility and modularity within the Network module.

### Includes:
```cpp
#include <asio.hpp>
```

### File Description:
The `IServer` file defines the `IServer` class, which outlines a single core function:
- `run`: Executes the server logic using an `asio::io_context` object.

### Class Details:
#### **Class: IServer**
```cpp
class IServer {
public:
    virtual void run(asio::io_context& io_context);
};
```

### Functionality:
- **`run(asio::io_context &io_context)`**
  - **Purpose:** Starts the server loop and manages its operations.
  - **Input:**
    - `asio::io_context &io_context`: Context object for managing asynchronous I/O.
  - **Behavior:** Abstract function to be overridden by the implementing server class. It is responsible for the main operational logic of the server.

### Example Usage:
This file only defines an interface and cannot be used directly. Below is a potential implementation in a derived class:
```cpp
class ConcreteServer : public IServer {
public:
    void run(asio::io_context &io_context) override {
        // Server logic here
        io_context.run();
    }
};
```

### Summary:
`IServer` establishes a standard for server implementation in the Network module. By defining the core `run` method, it ensures that any server implementation adheres to a consistent structure, fostering modularity and ease of integration.
