from typing import List, Optional, Tuple

Board = List[str]

def new_board() -> Board:
    return [" "] * 9

def show_board(b: Board) -> None:
    grid = [c if c != " " else str(i+1) for i, c in enumerate(b)]
    print("\n"
          f" {grid[0]} | {grid[1]} | {grid[2]}\n"
          "-----------\n"
          f" {grid[3]} | {grid[4]} | {grid[5]}\n"
          "-----------\n"
          f" {grid[6]} | {grid[7]} | {grid[8]}\n")

WIN_LINES = [
    (0,1,2),(3,4,5),(6,7,8),
    (0,3,6),(1,4,7),(2,5,8),
    (0,4,8),(2,4,6)
]
.
def winner(b: Board) -> Optional[str]:
    for a, c, d in WIN_LINES:
        if b[a] != " " and b[a] == b[c] == b[d]:
            return b[a]
    return None

def is_full(b: Board) -> bool:
    return all(c != " " for c in b)

def available_moves(b: Board) -> List[int]:
    return [i for i, c in enumerate(b) if c == " "]

def make_move(b: Board, idx: int, mark: str) -> Board:
    nb = b[:]
    nb[idx] = mark
    return nb

def minimax(b: Board, ai: str, human: str, is_ai_turn: bool) -> Tuple[int, Optional[int]]:
    w = winner(b)
    if w == ai:
        return 1, None
    if w == human:
        return -1, None
    if is_full(b):
        return 0, None

    best_move = None
    if is_ai_turn:
        best_score = -10
        for m in available_moves(b):
            score, _ = minimax(make_move(b, m, ai), ai, human, False)
            if score > best_score:
                best_score, best_move = score, m
        return best_score, best_move
    else:
        best_score = 10
        for m in available_moves(b):
            score, _ = minimax(make_move(b, m, human), ai, human, True)
            if score < best_score:
                best_score, best_move = score, m
        return best_score, best_move

def ask_move(b: Board, mark: str) -> int:
    while True:
        try:
            raw = input(f"Player {mark}, enter cell number (1-9): ").strip()
            idx = int(raw) - 1
            if idx not in range(9):
                print("✗ Number must be between 1 and 9")
                continue
            if b[idx] != " ":
                print("✗ That cell is already take.")
                continue
            return idx
        except ValueError:
            print("✗ Invalid input. Please enter a number between 1 and 9.")

def play_two_players() -> None:
    b = new_board()
    turn = "X"
    print("\n— Two Player Mode —")
    show_board(b)
    while True:
        idx = ask_move(b, turn)
        b[idx] = turn
        show_board(b)
        w = winner(b)
        if w:
            print(f"🎉 Winner: {w}")
            break
        if is_full(b):
            print("🤝 It's a tie!")
            break
        turn = "O" if turn == "X" else "X"

def play_vs_ai() -> None:
    b = new_board()
    print("\n— Play vs AI —")
    while True:
        human = input("Do you want to be X or O? (X/O): ").strip().upper()
        if human in ("X","O"):
            break
    ai = "O" if human == "X" else "X"

    while True:
        first = input("Who starts first? (H=You / A=AI): ").strip().upper()
        if first in ("H","A"):
            break

    turn = human if first == "H" else ai
    show_board(b)

    while True:
        if turn == human:
            idx = ask_move(b, human)
            b[idx] = human
        else:
            print("... AI is thinking ...")
            _, move = minimax(b, ai=ai, human=human, is_ai_turn=True)
            if move is None:
                moves = available_moves(b)
                move = moves[0] if moves else 0
            b[move] = ai
        show_board(b)

        w = winner(b)
        if w:
            if w == human:
                print("🎉 You win!")
            else:
                print("🤖 AI wins!")
            break
        if is_full(b):
            print("🤝 It's a tie!")
            break
        turn = ai if turn == human else human

def main():
    print("=*= Tic-Tac-Toe =*=")
    while True:
        print("\n1) Two Player Mode")
        print("2) Play vs AI")
        print("3) Exit")
        choice = input("Enter choice (1/2/3): ").strip()
        if choice == "1":
            play_two_players()
        elif choice == "2":
            play_vs_ai()
        elif choice == "3":
            break
        else:
            print("✗ Invalid choice.")

if __name__ == "__main__":
    main()
def main():
    print("=*= Tic-Tac-Toe =*=")
    while True:
        print("\n1) Two Player Mode")

        print("2) Play vs AI")
        print("3) Exit")
        choice = input("Enter choice (1/2/3): ").strip()
        if choice == "1":
            play_two_players()
        elif choice == "2":
            play_vs_ai()
        elif choice == "3":
            break
        else:
            print("✗ Invalid choice.")
            print("✗ Invalid choice.")
