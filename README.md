## Introduction to Entity-Component-System (ECS) in JavaScript with Functional Programming

Welcome to the world of game development! Today, we'll explore the basics of an Entity-Component-System (ECS) in JavaScript using a functional programming approach. This guide is perfect for beginners who want to understand the fundamentals of ECS and how to implement it using modern JavaScript (ES6).

### What is ECS?

ECS is a design pattern used in game development that provides a flexible way to manage entities, their properties, and behaviors. ECS breaks down into three main parts:
- **Entities**: The game objects (e.g., player, enemy).
- **Components**: Data containers that store properties (e.g., position, velocity).
- **Systems**: Logic that operates on entities with specific components (e.g., movement).

### Setting Up Components

Components are simple data objects. Let's start by creating two components: `Position` and `Velocity`.

```javascript
const createPositionComponent = (x, y) => ({ type: 'Position', x, y });

const createVelocityComponent = (vx, vy) => ({ type: 'Velocity', vx, vy });
```

### Creating Entities

An entity is a collection of components identified by a unique ID. We'll use a factory function to create entities and helper functions to manage components.

```javascript
const createEntity = (id) => ({
    id,
    components: []
});

const addComponent = (entity, component) => ({
    ...entity,
    components: [...entity.components, component]
});

const getComponent = (entity, componentType) =>
    entity.components.find(component => component.type === componentType);
```

### Building a Movement System

A system operates on entities with specific components. We'll create a `movementSystem` that updates entities based on their velocity.

```javascript
const movementSystem = (entities) =>
    entities.map(entity => {
        const position = getComponent(entity, 'Position');
        const velocity = getComponent(entity, 'Velocity');

        if (position && velocity) {
            const updatedPosition = { ...position, x: position.x + velocity.vx, y: position.y + velocity.vy };
            return {
                ...entity,
                components: entity.components.map(component =>
                    component.type === 'Position' ? updatedPosition : component
                )
            };
        }

        return entity;
    });
```

### Example Usage

Now, let's put everything together in an example.

```javascript
// Create some entities
let entity1 = createEntity(1);
entity1 = addComponent(entity1, createPositionComponent(0, 0));
entity1 = addComponent(entity1, createVelocityComponent(1, 1));

let entity2 = createEntity(2);
entity2 = addComponent(entity2, createPositionComponent(10, 10));
entity2 = addComponent(entity2, createVelocityComponent(-1, -1));

// Add entities to the system
const entities = [entity1, entity2];

// Update the system
console.log("Before update:");
entities.forEach(entity => {
    const position = getComponent(entity, 'Position');
    console.log(`Entity ${entity.id}: (${position.x}, ${position.y})`);
});

const updatedEntities = movementSystem(entities);

console.log("After update:");
updatedEntities.forEach(entity => {
    const position = getComponent(entity, 'Position');
    console.log(`Entity ${entity.id}: (${position.x}, ${position.y})`);
});
```

### Conclusion

Congratulations! You've just implemented a very basic ECS in JavaScript using functional programming. This pattern helps manage game objects and their behaviors efficiently. You can extend this framework by adding more components (like `HealthComponent`, `RenderComponent`) and systems (like `RenderingSystem`, `CollisionSystem`). Happy coding!
