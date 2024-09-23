
# AI Agent for Optimal Car Parking Sequence

## Overview

This AI agent is designed to solve a car parking optimization problem at a world-renowned car company. The problem arises when a saboteur scrambles employee cars, leaving them scattered across the parking area. Each car must be returned to its respective parking slot, but the process becomes challenging without an efficient plan. This AI agent aims to find the optimal sequence of actions to return all cars to their parking slots while minimizing the cost of moves.

## Problem Description

Employees at the company each have a personal car and a dedicated parking slot. The goal is to return each car to its assigned slot after they have been scrambled. The challenge involves navigating the cars on a grid while adhering to specific rules and minimizing costs associated with car movements. The problem can be likened to a sliding puzzle where only one car can move at a time.

### Grid Layout

The parking lot is represented as a grid of cells:
- Each car is represented by a letter (e.g., `A`, `B`, etc.).
- Each parking slot is represented by the index of the corresponding car in the English alphabet (e.g., `A -> 0`, `B -> 1`).
- Walls are represented by the character `#`, and vacant spaces by `.`.

An example parking layout:
```
#########
#1A...B0#
####.####
#########
```
- Car `A` needs to reach parking slot `0`.
- Car `B` needs to reach parking slot `1`.

### Rules
1. **Movement**: Cars can move in four directions—Up, Down, Left, or Right—but only into vacant cells (not walls or occupied cells).
2. **Action Cost**: The cost to move a car is based on the employee's rank (alphabetically):
   - Top-ranking employee (A) has a movement cost of 26.
   - Lowest-ranking employee (Z) has a movement cost of 1.
3. **Penalty for Parking Slot Violation**: If a car moves into another employee's parking slot, an additional penalty of 100 is applied.
   - For example, if car `A` moves into parking slot `1` (meant for car `B`), the move incurs a cost of `126` (26 for the move + 100 penalty).
4. **Goal**: The goal is to move each car to its respective parking slot with the minimum total cost.

### Solution Approach

A possible solution involves moving cars strategically, avoiding unnecessary penalties. For example, the sequence:
- Move car `A` out of the way so that car `B` can reach its slot.
- Then move car `A` into its slot.

An optimal sequence of actions ensures minimal total action cost, considering both movement and penalty costs.

## Example Solution

Given the example parking lot:
```
#########
#1A...B0#
####.####
#########
```
A solution would be:
1. Move car `A` right twice and down once: `[RIGHT, RIGHT, DOWN]`.
2. Move car `B` left five times to its parking slot: `[LEFT, LEFT, LEFT, LEFT, LEFT]`.
3. Finally, move car `A` up once and right three times: `[UP, RIGHT, RIGHT, RIGHT]`.

This solution requires 12 actions, with no violations of parking slots, minimizing the total cost.

## Implementation Details

The AI agent represents the parking lot as a grid of cells and performs search-based algorithms to find the optimal sequence of moves.

### Class: `ParkingProblem`

This class models the problem using key data structures and methods:

- **State Representation**: The state of the problem (`ParkingState`) is a tuple representing the position of each car on the grid. The parking lot has specific attributes like:
  - `passages`: A set of points indicating where a car can move (i.e., positions not occupied by walls).
  - `cars`: A tuple where each entry represents the position of a car.
  - `slots`: A dictionary mapping each parking slot to the corresponding car it belongs to.
  - `width` and `height`: The dimensions of the parking lot.

### Key Methods

1. **`get_initial_state(self) -> ParkingState`**  
   This method returns the initial state, which is the current position of all the cars in the parking lot.

2. **`is_goal(self, state: ParkingState) -> bool`**  
   This method checks if all cars are in their respective parking slots by comparing the current position of each car with its designated slot.

3. **`get_actions(self, state: ParkingState) -> List[ParkingAction]`**  
   This method generates all possible actions for a given state by checking which adjacent cells are valid moves (not walls or occupied by another car). Each action consists of a tuple indicating which car moves and the direction of the move.

4. **`get_successor(self, state: ParkingState, action: ParkingAction) -> ParkingState`**  
   This method returns the new state resulting from applying a given action to the current state. It updates the position of the car based on the direction specified in the action.

5. **`get_cost(self, state: ParkingState, action: ParkingAction) -> float`**  
   The cost function calculates the cost of performing an action, which depends on the employee’s rank and whether the action violates any parking slot rules:
   - The base cost is determined by the car's rank (A = 26, Z = 1).
   - An additional penalty of 100 is applied if a car enters another employee’s parking slot.

6. **`from_text(cls, text: str) -> 'ParkingProblem'`**  
   This static method reads a parking lot configuration from a text input. It parses the grid to identify car positions, parking slots, and passable areas. The grid contains:
   - `#` for walls.
   - Letters (A-Z) for cars.
   - Numbers (0-9) for parking slots.

7. **`from_file(cls, path: str) -> 'ParkingProblem'`**  
   Similar to `from_text`, this method reads a parking lot configuration from a file.

### Example Usage

```python
problem = ParkingProblem.from_file('path_to_file.txt')
initial_state = problem.get_initial_state()

# Run a search algorithm to find the solution
solution = search_algorithm(problem)

# Output the sequence of actions and the total cost
for action in solution:
    print(action)
```
## Usage

To run the AI agent, provide a parking lot configuration, and the agent will output the optimal sequence of moves and the corresponding total cost.

