# Name : KRISHNA KUMARI E
# REG NO: 212224060127
 # ExpNo:10 Implementation of Classical Planning Algorithm

# Aim

To implement a classical planning algorithm that, given an initial state, goal state, and a set of actions with their preconditions and effects, finds a sequence of actions (a plan) that transforms the initial state into the goal state.

# Algorithm or Steps Involved:
<ol>
  <li>Define the initial state</li>
  <li>Define the goal state</li>
  <li>Define the actions</li>
  <li>Find a <b>plan</b> to reach the goal state</li>
  <li>Print the plan</li>
</ol>

# Program
~~~
from collections import deque

def is_goal_state(state, goal_state):
    """Check if all goal conditions are met in the current state."""
    for key, value in goal_state.items():
        if state.get(key) != value:
            return False
    return True

def applicable(action, state):
    """Check if an action's precondition is satisfied by the current state."""
    for key, value in action['precondition'].items():
        if state.get(key) != value:
            return False
    return True

def apply_action(state, effect):
    """Apply the effect of an action to a copy of the state."""
    new_state = state.copy()
    for key, value in effect.items():
        new_state[key] = value
    return new_state

def find_plan(initial_state, goal_state, actions):
    """Find a sequence of actions from initial_state to goal_state using BFS."""
    # Use a queue for BFS: stores tuples of (current_state, plan_so_far)
    queue = deque()
    queue.append((initial_state, []))
    
    # To avoid revisiting states
    visited = set()
    visited.add(tuple(sorted(initial_state.items())))
    
    while queue:
        current_state, plan = queue.popleft()
        
        # Check goal
        if is_goal_state(current_state, goal_state):
            return plan
        
        # Try all applicable actions
        for action_name, action_data in actions.items():
            if applicable(action_data, current_state):
                new_state = apply_action(current_state, action_data['effect'])
                state_key = tuple(sorted(new_state.items()))
                if state_key not in visited:
                    visited.add(state_key)
                    queue.append((new_state, plan + [action_name]))
    
    return None  # No plan found
~~~

# Example - 1
```
initial_state = {'A': 'Table', 'B': 'Table'}
goal_state = {'A': 'B', 'B': 'Table'}

actions = {
    'move_A_to_B': {'precondition': {'A': 'Table', 'B': 'Table'}, 'effect': {'A': 'B'}},
    'move_B_to_Table': {'precondition': {'A': 'Table', 'B': 'B'}, 'effect': {'B': 'Table'}}
}

plan = find_plan(initial_state, goal_state, actions)
print(plan)
```
# Output:
```
['move_A_to_B']
```
# Example - 2
```
initial_state = {'A': 'Table', 'B': 'Table', 'C': 'Table'}
goal_state = {'A': 'B', 'B': 'C', 'C': 'Table'}

actions = {
    'move_A_to_B': {'precondition': {'A': 'Table', 'B': 'Table'}, 'effect': {'A': 'B'}},
    'move_B_to_C': {'precondition': {'A': 'B', 'B': 'Table', 'C': 'Table'}, 'effect': {'B': 'C'}},
    'move_C_to_Table': {'precondition': {'A': 'B', 'B': 'C', 'C': 'C'}, 'effect': {'C': 'Table'}}
}

plan = find_plan(initial_state, goal_state, actions)
print(plan)
```
# Output:
```
['move_A_to_B', 'move_B_to_C']
```
# Result:
  The program executed and verified successfully.
 
