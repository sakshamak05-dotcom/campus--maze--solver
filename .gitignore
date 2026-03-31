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
