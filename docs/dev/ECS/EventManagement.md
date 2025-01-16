# Event Management

Event management in the ECS architecture facilitates communication between systems and entities, enabling dynamic and decoupled game logic.

---

## Overview
Events represent actions or occurrences in the game, such as:
- **User Input:** Key presses or mouse clicks.
- **Collisions:** Two entities overlapping.
- **Timers:** Actions triggered after a set duration.

### Key Components
1. **Event:** Represents a single instance of an event.
2. **EventManager:** Handles the registration, dispatch, and processing of events.

---

## Event Class
The `Event` class encapsulates the details of an event.

### Implementation
```cpp
class Event {
public:
    enum EventType {
        COLLISION,
        KEY_PRESS,
        TIMER_EXPIRED
    };

    EventType type;
    size_t entity_id;
    size_t other_entity_id;

    Event(EventType type, size_t entity_id, size_t other_entity_id = 0)
        : type(type), entity_id(entity_id), other_entity_id(other_entity_id) {}
};
```

### Explanation
- **`type`**: Indicates the kind of event (e.g., collision).
- **`entity_id`**: The entity associated with the event.
- **`other_entity_id`**: An optional second entity, used for events like collisions.

---

## EventManager
The `EventManager` class oversees the lifecycle of events.

### Implementation
```cpp
class EventManager {
public:
    void add_event(const Event& event);
    std::vector<Event> get_events(Event::EventType type);
    void clear_events();

private:
    std::vector<Event> events;
};
```

### Methods
- **`add_event(const Event& event)`**
  - Adds a new event to the queue.
- **`get_events(Event::EventType type)`**
  - Retrieves all events of a specific type.
- **`clear_events()`**
  - Clears the event queue, usually after processing.

---

## Event Flow

### Workflow
1. **Event Creation:** A system or input handler generates an event.
2. **Event Dispatch:** The `EventManager` queues the event.
3. **Event Processing:** Relevant systems retrieve and handle events.

### Example
#### Collision Event
1. **Creation:** The Physics System detects a collision and generates a `COLLISION` event.
2. **Dispatch:** The event is added to the `EventManager`.
3. **Processing:**
   - The rendering system might display a visual effect.
   - The game logic system might reduce health or destroy an entity.

---

## Diagram
```plaintext
[System/Handler] ---> [EventManager]
        |                    |
        +--- Generates        +--- Dispatches
             Events                 Events
                                 |
                            [Processing Systems]
```

---

## File References
- [Systems](./Systems.md): Explore how systems interact with events.
- [Core Components](./CoreComponents.md): Key structures that work with events.

---

## Next Steps
Proceed to [System Manager](./SystemManager.md) for details on orchestrating systems and integrating event-driven logic.

