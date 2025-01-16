## Exception.md

### Purpose
The `Exception` class provides a mechanism for custom exception handling within the R-Type project. It is designed to simplify error reporting and debugging by allowing descriptive error messages to be propagated and displayed.

### Location
This file is part of the **Network** module in the R-Type project documentation.

### File Content

#### **Header File: Exception.hpp**
```cpp
#ifndef EXCEPTION_HPP_
#define EXCEPTION_HPP_

#include <iostream>
#include <exception>

namespace rtype {

    class Exception : public std::exception {
        public:
            Exception(const std::string &message) {
                this->_message = message;
            }
            ~Exception() noexcept = default;
            const char *what() const noexcept override { return _message.c_str(); }
        protected:
        private:
            std::string _message;
    };

}

#endif /* !EXCEPTION_HPP_ */
```

### Key Components

#### **Class: `Exception`**
- **Purpose:**
  The `Exception` class is a simple extension of the standard `std::exception` class, allowing for custom error messages.

- **Public Methods:**
  - `Exception(const std::string &message)`
    - Constructor to initialize the exception with a custom message.
  - `~Exception() noexcept`
    - Default destructor ensuring no exceptions are thrown during destruction.
  - `const char *what() const noexcept`
    - Overridden method to return the custom error message.

#### **Private Members:**
- `_message`
  - Stores the custom error message provided during the exception's instantiation.

### Example Usage
```cpp
#include "Exception.hpp"
#include <iostream>

int main() {
    try {
        throw rtype::Exception("A custom error occurred.");
    } catch (const rtype::Exception &e) {
        std::cerr << "Error: " << e.what() << std::endl;
    }
    return 0;
}
```
**Output:**
```
Error: A custom error occurred.
```

### Integration
The `Exception` class is used throughout the **Network** module to handle errors such as connection failures, invalid data, or server-related issues. By using this class, developers can provide detailed and consistent error reporting.

### Summary
- **Modularity:**
  - The `Exception` class encapsulates error handling, ensuring clean and maintainable code.
- **Extensibility:**
  - The class can be extended or modified to include additional features, such as error codes or logging.
- **Ease of Use:**
  - The API is straightforward, leveraging the familiar `std::exception` interface.
