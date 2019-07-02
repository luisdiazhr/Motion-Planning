# Motion Planning

The planning algorithm is implemented in motion_planning.py. It uses utility functions written in planning_utils.py. It basically performs the following steps: 

- Load the 2.5D map in the colliders.csv file describing the environment.
- Discretize the environment into a grid or graph representation.
- Define the start and goal locations.
- Perform a search using A* or other search algorithm.
- Use a collinearity test or ray tracing method (like Bresenham) to remove unnecessary waypoints.

The next sections explains how this is achieved.

## Reading Home Position
In order for the drone to start planning from anywhere, the drone has to read the global home location from the first line of the colliders.csv. file and set that position as global home. This is implemented at motion_planning.py line 127. It uses the function self.set_home_position().

## Taking off from Anywhere
In order for the drone to be able to takeoff from anywhere, I retrieve the global position using `global_position = [self._longitude, self._latitude, self._altitude]`. Then, I use the function global_to_local() to convert to local position `NED_local_position = global_to_local(global_position,self.global_home)`.

## Planning Start Point
The start point is the current local position. The grid start point is calculated in line 151.

## Setting a Goal Position
The goal coordinates can be hardcoded in line 158 .Then, they are transformed to local coordinates to use them in the searching algorithm 
`grid_goal_local = global_to_local(grid_goal,self.global_home)`.

## Search Algorithm
Diagonal motions are included to the A* implementation and have an assigned cost of sqrt(2). The diagonals movements are implemented by incorporating them in the Action enumerate. This requires a change in the method valid_actions() to account for those actions.
```python
    NORTH_WEST = (-1,-1, np.sqrt(2))
    NORTH_EAST = (-1, 1, np.sqrt(2))
    SOUTH_WEST = (1, -1, np.sqrt(2))
    SOUTH_EAST = (1,  1, np.sqrt(2))`
```    
## Path Pruning
The path is pruned at line 162 using collinearity (collinearity_prune function) with the method provided by the lectures.
