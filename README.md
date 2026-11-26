# EXPERIMENT NO. 9
# Solve Wumpus World Problem Using Python Demonstrating Inferences from Propositional Logic

**Name:** SANJAY A

**Register Number/Staff ID:** 212225040367

---

## Aim

To solve Wumpus World Problem using Python demonstrating Inferences from Propositional Logic.

---

# Problem Description

## Wumpus World

The Wumpus world is a simple world example to illustrate the worth of a knowledge-based agent and to represent knowledge representation.

The figure below shows a Wumpus world containing one pit and one Wumpus. There is an agent in room [1,1]. The goal of the agent is to exit the Wumpus world alive. The agent can exit the Wumpus world by reaching room [4,4]. The Wumpus world contains exactly one Wumpus and one pit. There will be a breeze in the rooms adjacent to the pit, and there will be a stench in the rooms adjacent to Wumpus.

### Wumpus World Representation

![Wumpus World Representation](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/cd6b68dc-c79f-4dcb-8126-04da90d65912)

*Wumpus World Representation*

This is a Python program that uses propositional logic sentences to check which rooms are safe.

It is assumed that there will always be a safe path that the agent can take to exit the Wumpus world. The logical agent can take four actions: Up, Down, Left and Right. These actions help the agent move from one room to an adjacent room. The agent can perceive two things: Breeze and Stench.

---

# Python Program

```python
# EXPERIMENT NO. 9
# WUMPUS WORLD USING PROPOSITIONAL LOGIC

# Importing required module
from itertools import permutations

# Initializing the Wumpus World
SIZE = 4

agent_position = (1, 1)
pit_position = (1, 3)
wumpus_position = (3, 4)
gold_position = (4, 4)

# Knowledge base
knowledge_base = []

# Visited locations
visited = {}

# Possible movement directions
directions = {
    'u': (-1, 0),
    'd': (1, 0),
    'l': (0, -1),
    'r': (0, 1)
}

# Function to find adjacent rooms
def get_neighbors(position):
    row, col = position
    neighbors = []

    for dr, dc in directions.values():
        new_row = row + dr
        new_col = col + dc

        if 1 <= new_row <= SIZE and 1 <= new_col <= SIZE:
            neighbors.append((new_row, new_col))

    return neighbors


# Function to identify percepts
def get_percepts(position):
    neighbors = get_neighbors(position)

    breeze = pit_position in neighbors
    stench = wumpus_position in neighbors

    return breeze, stench


# Propositional logic inference using model enumeration
def infer_safe_rooms():
    possible_models = []

    cells = [
        (r, c)
        for r in range(1, SIZE + 1)
        for c in range(1, SIZE + 1)
    ]

    for pit, wumpus in permutations(cells, 2):

        # The starting room cannot contain danger
        if pit == (1, 1) or wumpus == (1, 1):
            continue

        valid = True

        for position, (breeze, stench) in visited.items():

            neighbors = get_neighbors(position)

            # Check Breeze proposition
            if breeze != (pit in neighbors):
                valid = False
                break

            # Check Stench proposition
            if stench != (wumpus in neighbors):
                valid = False
                break

        if valid:
            possible_models.append((pit, wumpus))

    safe_rooms = set()

    if possible_models:
        for cell in cells:
            if all(
                cell != pit and cell != wumpus
                for pit, wumpus in possible_models
            ):
                safe_rooms.add(cell)

    return safe_rooms


# Function to update knowledge base
def update_knowledge(position):
    breeze, stench = get_percepts(position)

    visited[position] = (breeze, stench)

    knowledge_base.append(
        f"Room {position}: Breeze={breeze}, Stench={stench}"
    )

    print("\nCurrent location:", position)

    if position == gold_position:
        print("GOLD FOUND! You won....")
        print("Your score is:", 1000)
        return True

    if breeze and stench:
        print("Percepts: Breeze and Stench")
    elif breeze:
        print("Current location: Breeze")
    elif stench:
        print("Current location: Stench")
    else:
        print("Current location: Safe")

    safe_rooms = infer_safe_rooms()

    print("Safe neighbouring rooms:")

    for room in sorted(safe_rooms):
        if room in get_neighbors(position):
            print(room)

    return False


# Main program
print("WELCOME TO WUMPUS WORLD")
print("------------------------")

print("press u to move up")
print("press d to move down")
print("press l to move left")
print("press r to move right")
print("press q to quit")

# Observe the starting room
game_over = update_knowledge(agent_position)

# Movement loop
while not game_over:

    move = input("\nEnter your movement: ").lower().strip()

    if move == 'q':
        print("Game terminated.")
        break

    if move not in directions:
        print("Invalid input! Please enter u, d, l, r or q.")
        continue

    dr, dc = directions[move]

    new_position = (
        agent_position[0] + dr,
        agent_position[1] + dc
    )

    # Check grid boundaries
    if not (1 <= new_position[0] <= SIZE and
            1 <= new_position[1] <= SIZE):
        print("Invalid move! You cannot move outside the grid.")
        continue

    # Update agent position
    agent_position = new_position

    # Check for dangerous locations
    if agent_position == pit_position:
        print("\nYou fell into a PIT! Game Over.")
        break

    if agent_position == wumpus_position:
        print("\nYou encountered the WUMPUS! Game Over.")
        break

    # Update knowledge and check for gold
    game_over = update_knowledge(agent_position)
```

---

# Sample Input and Output

![Sample Input](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/8696111a-a4a7-47cb-ba4b-43a4ef88573f)

![Sample Output](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/4be5bf06-79fa-4fa0-9334-38a33f06060b)

---

# My Input and Output

## My Input

<img width="287" height="50" alt="image" src="https://github.com/user-attachments/assets/fed4a594-24fe-4fd1-b07f-6e2e8610968f" />




## My Output


<img width="185" height="837" alt="image" src="https://github.com/user-attachments/assets/37375092-37a9-4e7c-a1b7-94e3e692f62b" />



---

# Result

Thus, the Wumpus World Problem was successfully implemented using Python, demonstrating inferences from propositional logic.

---

**END OF EXPERIMENT NO. 9**
