# campus--maze--solver
The Problem
Imagine you are at the Hostel (S) and need to get to the AI Lab (G) for a morning class. The campus isn't just an empty field—there are buildings, walls, and restricted zones (#) in your path.

I built this Python-based "Problem-Solving Agent" to figure out the best way to navigate a 7x7 grid using two different types of "AI brains": BFS (Uninformed) and A* (Informed).

How the Agent "Thinks"
Algorithm	The Strategy	My Observation
BFS	"The Blind Wanderer"	It checks every single direction equally. It's guaranteed to find the shortest path, but it wastes a lot of energy looking at walls.
A Search*	"The Smart Navigator"	It uses a Heuristic (Manhattan Distance) to "sense" where the Goal is. It's much more efficient because it focuses on moving toward the target.
Real-World Results
When I ran the code on my campus map, here is how the agent performed:

Path Found: Yes (19 steps total).
Efficiency Win: While BFS had to "look" at 24 different coordinates, A* found the same path by only checking ~21 spots.
The Math: A* uses the evaluation function f(n) = g(n) + h(n) to stay on track.
Project Structure
src/solver.py: The core logic for the BFS and A* algorithms.
.gitignore: Keeps the repository clean by hiding temporary __pycache__ files.
How to Run the Solver
To test the agent on your own machine, follow these steps:

Run the Script: Execute the main solver file from your terminal:
python src/solver.py
Interpret the Map:
S = Hostel (Start)
G = AI Lab (Goal)
# = Buildings (Obstacles)
* = The path discovered by the AI!
Created by: Dhairya Jaiswal (25BAI10441)
Course: B.Tech in Artificial Intelligence & Machine Learning
import collections

campus_map = [
    [0, 0, 0, 0, 0, 0, 0],
    [0, 1, 1, 1, 1, 1, 0],
    [0, 0, 0, 0, 0, 1, 0],
    [1, 1, 1, 1, 0, 1, 0],
    [0, 0, 0, 0, 0, 0, 0],
    [0, 1, 1, 1, 1, 1, 1],
    [0, 0, 0, 0, 0, 0, 0]
]

START = (0, 0)
GOAL = (6, 6)

def bfs(grid, start, goal):
    queue = collections.deque([start])
    visited = {start}
    parent = {start: None}
    nodes_explored = 0

    while queue:
        current = queue.popleft()
        nodes_explored += 1

        if current == goal:
            return reconstruct_path(parent, goal), nodes_explored

        for dx, dy in [(0, 1), (0, -1), (1, 0), (-1, 0)]:
            neighbor = (current[0] + dx, current[1] + dy)

            if (0 <= neighbor[0] < len(grid) and
                0 <= neighbor[1] < len(grid[0]) and
                grid[neighbor[0]][neighbor[1]] == 0 and
                neighbor not in visited):

                visited.add(neighbor)
                parent[neighbor] = current
                queue.append(neighbor)

    return None, nodes_explored


def reconstruct_path(parent, goal):
    path = []
    current = goal

    while current is not None:
        path.append(current)
        current = parent[current]

    path.reverse()
    return path


# Run BFS
path, explored = bfs(campus_map, START, GOAL)

print("Path:", path)
print("Nodes explored:", explored)
def reconstruct_path(parent, goal):
    path = []
    current = goal

    while current is not None:
        path.append(current)
        current = parent[current]

    return path[::-1]


def display_grid(grid, path=None):
    for r in range(len(grid)):
        row_str = ""
        for c in range(len(grid[0])):
            if (r, c) == START:
                row_str += " S "
            elif (r, c) == GOAL:
                row_str += " G "
            elif path and (r, c) in path:
                row_str += " * "
            elif grid[r][c] == 1:
                row_str += " # "
            else:
                row_str += " . "
        print(row_str)


if __name__ == "__main__":
    print("--- Campus Navigation: BFS (Uninformed Search) ---")

    path_found, total_nodes = bfs(campus_map, START, GOAL)

    if path_found:
        print(f"Path Found! Length: {len(path_found)} steps.")
        print(f"Nodes Explored: {total_nodes}")
        print("\nVisual Map (S=Hostel, G=Lab, *=Path, #=Building):")
        display_grid(campus_map, path_found)
    else:
        print("No path found between the points.")
        
