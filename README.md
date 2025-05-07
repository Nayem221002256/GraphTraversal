# GraphTraversal

import random

def generate_grid(n):
    grid = []
    for i in range(n):
        row = []
        for j in range(n):
            row.append('.' if random.random() > 0.25 else '#')
        grid.append(row)
    return grid

def get_random_free_cell(grid):
    n = len(grid)
    free_cells = [(i, j) for i in range(n) for j in range(n) if grid[i][j] == '.']
    return random.choice(free_cells) if free_cells else None

def dfs(grid, source, goal):
    n = len(grid)
    stack = [source]
    visited = set()
    parent = {}
    topo_order = []

    while stack:
        current = stack.pop()
        if current in visited:
            continue
        visited.add(current)
        topo_order.append(current)

        if current == goal:
            break

        r, c = current
        for dr, dc in [(-1,0), (1,0), (0,-1), (0,1)]:
            nr, nc = r + dr, c + dc
            if 0 <= nr < n and 0 <= nc < n and grid[nr][nc] == '.' and (nr, nc) not in visited:
                stack.append((nr, nc))
                if (nr, nc) not in parent:
                    parent[(nr, nc)] = current

    path = []
    if goal in visited:
        node = goal
        while node != source:
            path.append(node)
            node = parent[node]
        path.append(source)
        path.reverse()
    return path, topo_order

def print_grid(grid, source, goal):
    for i, row in enumerate(grid):
        line = ''
        for j, cell in enumerate(row):
            if (i, j) == source:
                line += 'S '
            elif (i, j) == goal:
                line += 'G '
            else:
                line += cell + ' '
        print(line)

def main():
    n = random.randint(4, 7)
    grid = generate_grid(n)
    while True:
        source = get_random_free_cell(grid)
        goal = get_random_free_cell(grid)
        if source and goal and source != goal:
            break

    path, topo = dfs(grid, source, goal)

    print(f"\nGenerated Grid ({n}x{n}):")
    print_grid(grid, source, goal)
    print(f"\nSource: {source}")
    print(f"Goal: {goal}")
    if path:
        print("\nDFS Path:")
        print(path)
    else:
        print("\nNo path found using DFS.")
    print("\nTopological Order of DFS Traversal:")
    print(topo)

if __name__ == "__main__":
    main()
