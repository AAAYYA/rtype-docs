# Installation and Setup

## System Requirements
To successfully install and run the project, ensure your system meets the following requirements:

- **Operating System**: Linux, macOS, or Windows (Windows Subsystem for Linux recommended for Windows users).
- **Compiler**: GCC 9.3 or higher, or Clang 11.0 or higher.
- **Dependencies**:
  - CMake 3.15 or higher
  - Make
  - Raylib library
  - ASIO library
  - JSON for Modern C++
  - Network utilities (for socket programming)
- **Hardware**: At least 2GB of free RAM and a dual-core processor.

## Installation Steps

### 1. Clone the Repository
Download the project from the repository using Git:
```bash
$ git clone https://github.com/your-repo/rtype-project.git
$ cd rtype-project
```

### 2. Install Dependencies
Ensure all dependencies are installed on your system:

#### On Linux:
```bash
$ sudo apt update
$ sudo apt install build-essential cmake libasio-dev libraylib-dev libjsoncpp-dev
```

#### On macOS:
```bash
$ brew update
$ brew install cmake raylib asio jsoncpp
```

#### On Windows:
- Install a compatible compiler such as MinGW or MSVC.
- Use a package manager like `vcpkg` to install libraries:
  ```bash
  vcpkg install asio raylib jsoncpp
  ```

### 3. Build the Project
Create a build directory and compile the project using CMake:
```bash
$ mkdir build && cd build
$ cmake ..
$ make
```

### 4. Run the Application
To launch the server:
```bash
$ ./rtype-server
```
To launch the client:
```bash
$ ./rtype-client
```

## First Run
1. Start the server:
   ```bash
   $ ./rtype-server
   ```
   - The server will initialize and begin listening for connections on the default port.

2. Start the client:
   ```bash
   $ ./rtype-client
   ```
   - Enter the server's IP address and your username to connect.

3. Verify the connection:
   - The client should display a successful connection message.
   - The server console should log the new connection.

### Troubleshooting
- If you encounter issues:
  - Ensure all dependencies are installed.
  - Check that the server is running and reachable from the client.
  - Verify the firewall settings to allow traffic on the specified ports.

If you experience further issues, consult the documentation or reach out to the support team.

