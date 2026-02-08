import math

board = [" " for _ in range(9)]

def print_board():
    print()
    for i in range(3):
        print(" | ".join(board[i*3:(i+1)*3]))
        if i < 2:
            print("---------")
    print()

def winner(b, player):
    win_positions = [
        [0,1,2],[3,4,5],[6,7,8],
        [0,3,6],[1,4,7],[2,5,8],
        [0,4,8],[2,4,6]
    ]
    return any(all(b[i] == player for i in combo) for combo in win_positions)

def is_draw():
    return " " not in board

def minimax(b, depth, is_max):
    if winner(b, "O"):
        return 1
    if winner(b, "X"):
        return -1
    if " " not in b:
        return 0

    if is_max:
        best = -math.inf
        for i in range(9):
            if b[i] == " ":
                b[i] = "O"
                best = max(best, minimax(b, depth+1, False))
                b[i] = " "
        return best
    else:
        best = math.inf
        for i in range(9):
            if b[i] == " ":
                b[i] = "X"
                best = min(best, minimax(b, depth+1, True))
                b[i] = " "
        return best

def ai_move():
    best_score = -math.inf
    move = 0
    for i in range(9):
        if board[i] == " ":
            board[i] = "O"
            score = minimax(board, 0, False)
            board[i] = " "
            if score > best_score:
                best_score = score
                move = i
    board[move] = "O"

def player_move():
    while True:
        move = int(input("Choose position (1-9): ")) - 1
        if 0 <= move <= 8 and board[move] == " ":
            board[move] = "X"
            break
        else:
            print("Invalid move, try again.")

# Game loop
print("AI Tic Tac Toe 🤖 (You = X, AI = O)")
print_board()

while True:
    player_move()
    print_board()
    if winner(board, "X"):
        print("You win 😲 (Impossible!)")
        break
    if is_draw():
        print("Draw!")
        break

    print("AI is thinking...")
    ai_move()
    print_board()
    if winner(board, "O"):
        print("AI wins 🤖🔥")
        break
    if is_draw():
        print("Draw!")
        break
