# Motion Planning

The planning algorithm is implemented in motion_planning.py. It uses utility functions written in planning_utils.py. It basically performs the following steps: 

- Load the 2.5D map in the colliders.csv file describing the environment.
- Discretize the environment into a grid or graph representation.
- Define the start and goal locations.
- Perform a search using A* or other search algorithm.
- Use a collinearity test or ray tracing method (like Bresenham) to remove unnecessary waypoints.

The next sections explains how this is achieved.

## Reading Home Position
In order for the drone to start planning from anywhere, the drone has to read the global home location from the first line of the colliders.csv. file and set that position as global home (self.set_home_position()). This is implemented at motion_planning.py line 124. It uses the function read_global_home() added to planning_utils.py.

## Taking off from Anywhere
In order for the drone to be able to takeoff from anywhere, I use the utility function global_to_local() to convert to local position (using self.global_home() as well, which you just set). This is done at line 130.

## Planning Start Point
In the starter code, the start point for planning is hardcoded as map center. Change this to be your current local position.
The grid star point is calculated from line 144 to 146.

## Setting a Goal Position
Goal coordinates can be set at line .Then, they are to local coordinates at lines 151 to 152 to be used on the search algorithm.

## Search Algorithm
Diagonal motions are included to the A* implementation and have an assigned cost of sqrt(2). The diagonals movements are implemented by incorporating them in the Action enumerate. This requires a change in the method valid_actions() to account for those actions. Here is an example of the A* trajectories on a grid:

## Path Pruning
The path is pruned at line 162 using collinearity (collinearity_prune function) with the method provided by the lectures.
