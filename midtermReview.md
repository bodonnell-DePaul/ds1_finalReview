# ds1_midtermReview

## 1. Recursive Path Finding - Write a recursive method that finds all possible paths from the top-left corner to the bottom-right corner of an n×m grid, where you can only move right or down.

# Recursive Path Finding: Implementation Instructions

## Problem Understanding
Your task is to find all possible paths from the top-left corner (0,0) to the bottom-right corner of an n×m grid, where you can only move right or down, avoiding obstacles marked as 1 in the grid.

## Implementation Guide (Without Code)

### Step 1: Set Up the Problem Structure
- Create a class to contain your solution
- Define a method that takes a 2D grid as input and returns a collection of possible paths
- Create a helper recursive method that will explore the paths

### Step 2: Define the Recursive Method Parameters
Your recursive method should track:
- Current position (row and column)
- Path taken so far (can be represented as a string of 'R's and 'D's)
- A collection to store all valid paths found

### Step 3: Implement Base Cases
Your recursive method should handle these cases:
1. **Invalid Move Check**: Return if the current position is:
   - Out of bounds (beyond grid dimensions)
   - An obstacle cell (value is 1)

2. **Destination Reached Check**: If at the bottom-right corner:
   - Add the current path to your collection of valid paths
   - Return from the recursive call

### Step 4: Recursive Exploration
From the current valid position, make recursive calls to explore:
1. Moving right (increasing column)
2. Moving down (increasing row)
- For each direction, append the corresponding move ('R' or 'D') to the current path


Remember to verify both the number of paths and that each path indeed avoids all obstacles and reaches the destination.

# Grids with Obstacles for Path Finding Problem

Here are several grid examples with obstacles of different sizes:

## 3×3 Grids with Obstacles

### Grid 1
```
[
  [0, 0, 0],
  [0, 1, 0],  // Center obstacle
  [0, 0, 0]
]
```

### Grid 2
```
[
  [0, 1, 0],  // Obstacle in top row
  [0, 0, 0],
  [0, 0, 0]
]
```

### Grid 3
```
[
  [0, 0, 0],
  [0, 0, 0],
  [0, 1, 0]   // Obstacle in bottom row
]
```

### Grid 4
```
[
  [0, 0, 0],
  [1, 1, 0],  // Multiple obstacles blocking a path
  [0, 0, 0]
]
```

## 3×4 Grids with Obstacles

### Grid 1
```
[
  [0, 0, 0, 0],
  [0, 1, 1, 0],  // Line of obstacles in middle row
  [0, 0, 0, 0]
]
```

### Grid 2
```
[
  [0, 0, 0, 0],
  [0, 1, 0, 0],
  [0, 0, 1, 0]   // Diagonal obstacles
]
```

### Grid 3
```
[
  [0, 1, 0, 0],
  [0, 0, 1, 0],
  [0, 0, 0, 0]   // Zigzag pattern of obstacles
]
```

### Grid 4
```
[
  [0, 0, 0, 1],
  [0, 1, 0, 0],
  [0, 0, 0, 0]   // Scattered obstacles
]
```

## 4×4 Grids with Obstacles

### Grid 1
```
[
  [0, 0, 0, 0],
  [0, 1, 1, 0],
  [0, 1, 0, 0],
  [0, 0, 0, 0]   // L-shaped obstacle
]
```

### Grid 2
```
[
  [0, 0, 0, 0],
  [0, 1, 0, 0],
  [0, 0, 1, 0],
  [0, 0, 0, 0]   // Diagonal obstacles
]
```

### Grid 3
```
[
  [0, 0, 0, 0],
  [1, 1, 0, 0],
  [0, 0, 0, 1],
  [0, 0, 0, 0]   // Multiple scattered obstacles
]
```

### Grid 4
```
[
  [0, 1, 0, 0],
  [0, 0, 0, 0],
  [0, 1, 1, 0],
  [0, 0, 0, 0]   // Multiple obstacles creating a more complex path
]
```

### Grid 5 (Challenging)
```
[
  [0, 0, 0, 0],
  [1, 1, 1, 0],
  [0, 0, 0, 0],
  [0, 1, 1, 0]   // Two barrier obstacles forcing a specific path
]
```

When implementing the solution, you'll need to add logic to check if a cell contains an obstacle (1) before attempting to include it in a path. A cell with an obstacle should be considered invalid for path traversal.

# Implementing an Undo Function Using a Stack

## Conceptual Understanding

An "undo" functionality is a common feature in applications where users can revert their most recent actions. A stack is the perfect data structure for this because it follows the Last-In-First-Out (LIFO) principle, which matches exactly how undo operations work.

## Design Approach

### Step 1: Define Command Objects
Create a uniform way to represent different types of actions in your application. Each command should:
- Encapsulate all information needed to execute an action
- Store any information needed to reverse the action
- Provide methods to execute and undo the action

### Step 2: Set Up the Stack Structure
- Create a stack to store executed commands
- Each time a user performs an action, push the corresponding command onto the stack
- When "undo" is requested, pop the most recent command and execute its undo logic

### Step 3: Implement Command Interface/Class
Design a command structure that can represent various application actions. Consider:
- What parameters each command needs
- How to store the previous state for reversal
- How to implement both "do" and "undo" functionality

### Step 4: Handle Edge Cases
- Empty stack (no actions to undo)
- Stack size limitation (memory considerations)
- Potential for undo chain breaking

## Testing Approach
- Test simple undo scenarios (single action undone)
- Test multiple sequential undos
- Test boundary conditions (no actions to undo)
- Verify that application state is correctly restored after each undo

## Mock Command List Example

Here's a conceptual list of commands for a text editor application:

1. `InsertTextCommand`
   - Stores: position, inserted text
   - Do: Insert text at position
   - Undo: Delete text from position

2. `DeleteTextCommand`
   - Stores: position, deleted text
   - Do: Delete text at position
   - Undo: Insert the deleted text back at position

3. `FormatTextCommand`
   - Stores: text range, old formatting, new formatting
   - Do: Apply new formatting to range
   - Undo: Apply old formatting to range

4. `MoveTextCommand`
   - Stores: source position, destination position, text
   - Do: Move text from source to destination
   - Undo: Move text from destination back to source

5. `ReplaceTextCommand`
   - Stores: position, old text, new text
   - Do: Replace old text with new text
   - Undo: Replace new text with old text

## Algorithm Sequence

1. User performs action → Create appropriate Command object → Execute command → Push to stack
2. User clicks Undo → Pop most recent Command from stack → Execute its undo method
3. Application state is reverted to before the command was executed

---
# Implementing a Call Center Queue

## Conceptual Understanding

A call center queue is a system that manages incoming customer calls by placing them in a waiting line until an agent becomes available. This is a perfect application for the Queue data structure, which follows the First-In-First-Out (FIFO) principle - the first caller to enter the queue will be the first one attended to.

## Design Approach

### Step 1: Define the Caller Data Structure
Create a structure to represent callers with relevant information such as:
- Caller ID
- Phone number
- Time when call was received
- Priority level (optional)
- Issue category or reason for call

### Step 2: Set Up the Queue Structure
- Create a queue to store callers waiting to be served
- Include methods to add new callers to the queue
- Include methods to remove callers when they're assigned to an agent
- Consider adding a method to display queue status

### Step 3: Implement Agent Management
- Maintain a collection of available agents
- Track agent status (available, busy)
- Implement a method to assign calls to available agents

## Mock Call Center Data

### Callers
1. `Caller #1001`
   - Phone: 555-123-4567
   - Time: 10:15 AM
   - Issue: Technical Support
   - Priority: Normal

2. `Caller #1002`
   - Phone: 555-234-5678
   - Time: 10:17 AM
   - Issue: Billing Question
   - Priority: Normal

3. `Caller #1003`
   - Phone: 555-345-6789
   - Time: 10:20 AM
   - Issue: Account Access
   - Priority: High

4. `Caller #1004`
   - Phone: 555-456-7890
   - Time: 10:22 AM
   - Issue: Product Information
   - Priority: Low

5. `Caller #1005`
   - Phone: 555-567-8901
   - Time: 10:25 AM
   - Issue: Technical Support
   - Priority: Normal

### Agents
1. `Agent #101 (Sarah)`
   - Specialization: Technical Support
   - Status: Available

2. `Agent #102 (Michael)`
   - Specialization: Billing
   - Status: Busy until 10:30 AM

3. `Agent #103 (Jennifer)`
   - Specialization: General Support
   - Status: Available

## Algorithm Sequence

1. New call arrives → Create Caller object → Add to queue
2. Agent becomes available → Remove first caller from queue → Assign to agent
3. If priority call arrives → Insert in appropriate position based on priority
4. If call is abandoned → Remove from queue
5. Periodically recalculate estimated wait times


---

# Video Game Inventory System Using Custom Doubly Linked List

## Conceptual Understanding

A video game inventory system organizes and manages various items a player collects during gameplay. Using a custom doubly linked list implementation allows for efficient navigation in both directions through the inventory categories and items.

## System Design Overview

### Step 1: Define the Item Structure
Create a class to represent individual inventory items with properties such as:
- Item ID
- Name
- Description
- Value/Cost
- Rarity level
- Stats/Effects
- Icon/Visual representation

### Step 2: Design the Doubly Linked List Node
Create a node structure that contains:
- Reference to the item data
- Pointer to the next node
- Pointer to the previous node

### Step 3: Implement the Doubly Linked List
Create a custom doubly linked list class with operations:
- Add item (at beginning, end, or specific position)
- Remove item
- Find item
- Navigate forward and backward
- Get list size
- Check if empty

### Step 4: Create Category Management
Organize items by category:
- Define an enum or constants for item categories (Weapons, Armor, Consumables, Quest Items, etc.)
- Create a collection of doubly linked lists - one for each category
- Implement methods to access specific category lists

### Step 5: Design Inventory Manager
Create a manager class that:
- Contains all category lists
- Provides methods to add/remove items to appropriate categories
- Handles inventory limitations (weight, slot count, etc.)
- Implements sorting capabilities (by value, name, etc.)
- Offers filtering options

### Step 6: User Interface Considerations
- Design methods to display the current category and items
- Implement navigation between categories
- Create methods for item selection, use, and transfer

## Mock Inventory Data

### Weapons Category
1. **Iron Sword**
   - ID: W001
   - Description: "A basic iron sword"
   - Value: 50 gold
   - Stats: 10 damage
   - Rarity: Common

2. **Fire Bow**
   - ID: W002
   - Description: "A bow imbued with fire magic"
   - Value: 250 gold
   - Stats: 15 damage, 5 fire damage
   - Rarity: Uncommon

3. **Dragon Slayer Axe**
   - ID: W003
   - Description: "Legendary axe used to slay dragons"
   - Value: 1500 gold
   - Stats: 50 damage, +30% damage to dragons
   - Rarity: Rare

### Armor Category
1. **Leather Helmet**
   - ID: A001
   - Description: "Basic protection for your head"
   - Value: 35 gold
   - Stats: 5 defense
   - Rarity: Common

2. **Steel Chestplate**
   - ID: A002
   - Description: "Solid chest protection"
   - Value: 120 gold
   - Stats: 20 defense
   - Rarity: Common

3. **Enchanted Boots**
   - ID: A003
   - Description: "Boots with a speed enchantment"
   - Value: 300 gold
   - Stats: 8 defense, +10% movement speed
   - Rarity: Uncommon

### Consumables Category
1. **Health Potion**
   - ID: C001
   - Description: "Restores 50 health"
   - Value: 25 gold
   - Effect: +50 HP
   - Rarity: Common

2. **Mana Elixir**
   - ID: C002
   - Description: "Restores 75 mana"
   - Value: 40 gold
   - Effect: +75 MP
   - Rarity: Common

3. **Strength Potion**
   - ID: C003
   - Description: "Temporarily increases strength"
   - Value: 100 gold
   - Effect: +20% strength for 60 seconds
   - Rarity: Uncommon

## Implementation Approach

1. First implement the Item class with all necessary properties
2. Create the Node class for the doubly linked list
3. Implement the DoublyLinkedList class with all required operations
4. Create the CategoryManager to handle multiple lists
5. Develop the InventoryManager as the main interface
6. Test with sample items across different categories
7. Add more advanced features like sorting and filtering

---

## Create a HashMap from the Baseball dataset we used last week.  The key in the dictionary should be the Team name and the value associated with the key should only be the players on that team who got at least 1 hit