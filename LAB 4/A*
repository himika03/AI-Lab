from heapq import heappush, heappop


goal_state = [
    [1, 2, 3],
    [8, 0, 4],
    [7, 6, 5]
]


directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]
direction_names = ["UP", "DOWN", "LEFT", "RIGHT"]

def misplaced_tiles(state):
    count = 0
    for i in range(3):
        for j in range(3):
            if state[i][j] != 0 and state[i][j] != goal_state[i][j]:
                count += 1
    return count

def manhattan_distance(state):
    distance = 0
    for i in range(3):
        for j in range(3):
            tile = state[i][j]
            if tile != 0:
                goal_x, goal_y = divmod(tile - 1, 3)
                distance += abs(i - goal_x) + abs(j - goal_y)
    return distance

def get_neighbors_with_actions(state):
    neighbors = []
    for i in range(3):
        for j in range(3):
            if state[i][j] == 0:
                x, y = i, j
                break

    for (dx, dy), action in zip(directions, direction_names):
        nx, ny = x + dx, y + dy
        if 0 <= nx < 3 and 0 <= ny < 3:
            new_state = [list(row) for row in state]
            new_state[x][y], new_state[nx][ny] = new_state[nx][ny], new_state[x][y]
            neighbors.append((new_state, action))
    return neighbors

def state_to_tuple(state):
    return tuple(tuple(row) for row in state)

def reconstruct_path(came_from, current):
    actions = []
    states = []
    while current in came_from:
        prev_state, action = came_from[current]
        actions.append(action)
        states.append(current)
        current = prev_state
    states.append(current)  
    actions.reverse()
    states.reverse()
    return actions, states

def a_star_search_with_steps(initial_state, heuristic_func):
    open_list = []
    closed_set = set()

    g_score = {state_to_tuple(initial_state): 0}
    f_score = {state_to_tuple(initial_state): heuristic_func(initial_state)}
    came_from = {}

    heappush(open_list, (f_score[state_to_tuple(initial_state)], initial_state))

    while open_list:
        _, current_state = heappop(open_list)
        current_t = state_to_tuple(current_state)

        if current_state == goal_state:
            return reconstruct_path(came_from, current_t)

        closed_set.add(current_t)

        for neighbor, action in get_neighbors_with_actions(current_state):
            neighbor_t = state_to_tuple(neighbor)
            if neighbor_t in closed_set:
                continue

            tentative_g = g_score[current_t] + 1
            if neighbor_t not in g_score or tentative_g < g_score[neighbor_t]:
                came_from[neighbor_t] = (current_t, action)
                g_score[neighbor_t] = tentative_g
                f_score[neighbor_t] = tentative_g + heuristic_func(neighbor)
                heappush(open_list, (f_score[neighbor_t], neighbor))

    return None, None

def print_path(actions, states):
    for i, (action, state) in enumerate(zip(actions, states[1:]), 1):
        print(f"Step {i}: {action}")
        for row in state:
            print(row)
        print()


initial_state = [
    [2, 8, 3],
    [1, 6, 4],
    [7, 0, 5]
]


print("Using Misplaced Tiles heuristic:")
actions, states = a_star_search_with_steps(initial_state, misplaced_tiles)
if actions:
    print_path(actions, states)
    print("Total steps:", len(actions))
else:
    print("No solution found.")


print("\nUsing Manhattan Distance heuristic:")
actions, states = a_star_search_with_steps(initial_state, manhattan_distance)
if actions:
    print_path(actions, states)
    print("Total steps:", len(actions))
else:
    print("No solution found.")
