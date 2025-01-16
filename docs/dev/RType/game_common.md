## GameCommon.hpp

### Purpose:
The `GameCommon` file provides utility functions for rendering health bars, power-up bars, and other shared gameplay elements. These functions are essential for maintaining consistent and reusable UI components across the game.

### Includes:
```cpp
#include <raylib.h>
#include <iostream>
```

### File Description:
This file contains utility functions that assist in rendering graphical elements related to game state, such as health bars and power-up bars. These utilities are designed to be simple and reusable, enabling consistent visuals throughout the game.

### Key Functions:

1. **draw_health_bar(float x, float y, float width, float height, int current_health, int max_health):**
   - Draws a health bar at the specified position and size, reflecting the entity's current health.
   - Health bar color changes dynamically based on the percentage of remaining health.

   **Parameters:**
   - `x, y`: Position of the health bar.
   - `width, height`: Dimensions of the health bar.
   - `current_health, max_health`: Current and maximum health values.

   **Example Usage:**
   ```cpp
   draw_health_bar(20, 20, 200, 20, playerHealth.current, playerHealth.max);
   ```

2. **draw_power_up_bar(float x, float y, float width, float height, float progress, bool isActive):**
   - Draws a progress bar to indicate the status of a power-up.
   - Bar color changes based on the activation state of the power-up.

   **Parameters:**
   - `x, y`: Position of the power-up bar.
   - `width, height`: Dimensions of the power-up bar.
   - `progress`: Progress ratio (0.0 to 1.0).
   - `isActive`: Boolean indicating if the power-up is active.

   **Example Usage:**
   ```cpp
   draw_power_up_bar(20, 50, 200, 20, powerUpProgress, powerUpActive);
   ```

### Collaboration:
This file interacts with:
- **Rendering Systems:** To display health and power-up bars during gameplay.
- **Game State:** Uses values like health and progress provided by components or systems.

### Design Considerations:
- **Reusability:**
  - The functions are designed to be generic and adaptable for various entities and scenarios.
- **Performance:**
  - Minimal performance impact by relying on efficient drawing calls provided by Raylib.
- **Customization:**
  - Easily extendable to include additional UI elements or adapt to different visual styles.

### Summary:
`GameCommon.hpp` serves as a collection of utility functions that simplify the rendering of critical gameplay UI elements like health and power-up bars. Its focus on reusability and simplicity ensures consistent and efficient integration across game systems.
