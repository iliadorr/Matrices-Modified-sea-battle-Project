# Matrices-Modified-sea-battle-Project
Homework for Mr Tauheed
import os
import random

# Function to print the game board
def print_board(board):
    os.system("cls" if os.name == "nt" else "clear")
    print("   A B C D E F G")
    print("  ----------------")
    for i in range(7):
        print(f"{i + 1}|", end=" ")
        for j in range(7):
            print(board[i][j], end=" ")
        print()

# Function to check if a ship can be placed at the specified coordinates
def can_place_ship(board, ship, row, col, direction):
    size = len(ship)
    if direction == "H":
        if col + size > 7:
            return False
        for j in range(col, col + size):
            if board[row][j] != " ":
                return False
    elif direction == "V":
        if row + size > 7:
            return False
        for i in range(row, row + size):
            if board[i][col] != " ":
                return False
    return True

# Function to place ships on the board
def place_ships(board):
    ships = [3, 2, 2, 1, 1, 1, 1]
    for ship in ships:
        placed = False
        while not placed:
            row = random.randint(0, 6)
            col = random.randint(0, 6)
            direction = random.choice(["H", "V"])
            if can_place_ship(board, range(ship), row, col, direction):
                if direction == "H":
                    for j in range(col, col + ship):
                        board[row][j] = str(ship)
                elif direction == "V":
                    for i in range(row, row + ship):
                        board[i][col] = str(ship)
                placed = True

# Function to update the board after a shot
def update_board(board, row, col):
    if board[row][col] == " ":
        board[row][col] = "M"  # Mark as miss
    elif board[row][col].isdigit():
        ship_size = int(board[row][col])
        board[row][col] = "H"  # Mark as hit
        if all("H" in row for row in board) or all("H" in col for col in zip(*board)):
            print_board(board)
            print("You sunk a ship!")
        elif str(ship_size) not in [cell for row in board for cell in row]:
            print_board(board)
            print("You sunk a ship!")
    else:
        print_board(board)
        print("You already shot there!")

# Function to play the game
def play_game():
    board = [[" " for _ in range(7)] for _ in range(7)]
    place_ships(board)
    print("Welcome to Battleship!")
    print_board(board)
    shots = 0

    while any(cell.isdigit() for row in board for cell in row):
        try:
            shot = input("Enter your shot (e.g., A3): ")
            col = ord(shot[0].upper()) - ord("A")
            row = int(shot[1]) - 1

            if 0 <= row < 7 and 0 <= col < 7:
                shots += 1
                update_board(board, row, col)
            else:
                print("Invalid shot! Please enter a valid shot.")
        except (ValueError, IndexError):
            print("Invalid input! Please enter a valid shot.")

    print("Congratulations! You sunk all the ships!")
    print(f"Number of shots: {shots}")

# Main game loop
while True:
    play_game()
    play_again = input("Do you want to play again? (yes/no): ").lower()
    if play_again != "yes":
        print("Thanks for playing! Here is the sorted list of players.")
        # Your code to display the sorted list of players goes here
        break
