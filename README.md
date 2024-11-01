Here's an updated version of the code with two interactive modes:

1. **Play against the AI** - You take turns with the AI, which uses the minimax strategy to select its moves.
2. **Play against another player** - Allows two players to take turns without AI intervention.

---

```python
import copy

class Nim:
    def __init__(self, pile1, pile2):
        # Initialize the game with two piles of specified sizes
        self.pile1 = pile1
        self.pile2 = pile2

    def is_terminal(self):
        # The game is over if one of the piles is empty
        return self.pile1 == 0 or self.pile2 == 0

    def get_possible_moves(self):
        # Generate all possible moves (remove 1 to all matches from either pile)
        moves = []
        for i in range(1, self.pile1 + 1):
            moves.append((1, i))  # Remove i matches from pile 1
        for j in range(1, self.pile2 + 1):
            moves.append((2, j))  # Remove j matches from pile 2
        return moves

    def make_move(self, move):
        # Apply a move to the current game state
        pile, matches = move
        if pile == 1:
            self.pile1 -= matches
        elif pile == 2:
            self.pile2 -= matches

    def display_state(self):
        # Display the current state of the piles
        print(f"Piles: {self.pile1} and {self.pile2}")

def minimax(game, is_current_player_turn):
    # Base case: if the game is over, return -1 if it's the current player's turn (loss), +1 otherwise (win)
    if game.is_terminal():
        return -1 if is_current_player_turn else 1

    # Initialize the best score based on whose turn it is
    if is_current_player_turn:
        best_score = float('-inf')  # Maximizing player’s best score
    else:
        best_score = float('inf')   # Minimizing player’s best score

    # Loop over each possible move
    for move in game.get_possible_moves():
        # Step 1: Create a deep copy of the game state
        new_game = copy.deepcopy(game)

        # Step 2: Apply the move on the copied game state
        new_game.make_move(move)

        # Step 3: Recursively call minimax on the new game state for the opponent's turn
        score = minimax(new_game, not is_current_player_turn)

        # Step 4: Update best_score based on maximizing or minimizing
        if is_current_player_turn:
            best_score = max(best_score, score)
        else:
            best_score = min(best_score, score)

    return best_score

def minimax_solver(game):
    # Find the best move for the current player (assuming it’s their turn)
    best_score = float('-inf')
    best_move = None
    for move in game.get_possible_moves():
        # Create a deep copy of the game state and apply the move
        new_game = copy.deepcopy(game)
        new_game.make_move(move)

        # Evaluate this move using minimax
        score = minimax(new_game, False)  # Assume opponent's turn next

        # Update the best score and best move
        if score > best_score:
            best_score = score
            best_move = move

    return best_move

# Play against AI mode
def play_against_ai():
    game = Nim(5, 5)  # Start with 5 matches in each pile
    print("Welcome to Nim! You’re playing against the AI.")
    game.display_state()

    while not game.is_terminal():
        # Player's turn
        print("\nYour Turn:")
        pile = int(input("Choose pile (1 or 2): "))
        matches = int(input("Choose number of matches to remove: "))
        game.make_move((pile, matches))
        game.display_state()
        if game.is_terminal():
            print("Congratulations! You won!")
            break

        # AI's turn
        print("\nAI's Turn:")
        ai_move = minimax_solver(game)
        game.make_move(ai_move)
        print(f"AI chose to remove {ai_move[1]} matches from pile {ai_move[0]}.")
        game.display_state()
        if game.is_terminal():
            print("The AI won! Better luck next time.")
            break

# Play against another player mode
def play_against_player():
    game = Nim(5, 5)  # Start with 5 matches in each pile
    print("Welcome to Nim! Two players will take turns.")
    game.display_state()

    current_player = 1
    while not game.is_terminal():
        print(f"\nPlayer {current_player}'s Turn:")
        pile = int(input("Choose pile (1 or 2): "))
        matches = int(input("Choose number of matches to remove: "))
        game.make_move((pile, matches))
        game.display_state()
        
        if game.is_terminal():
            print(f"Player {current_player} wins! Congratulations!")
            break

        # Switch player
        current_player = 2 if current_player == 1 else 1

# Main function to choose game mode
def main():
    print("Choose a game mode:")
    print("1. Play against AI")
    print("2. Play against another player")
    choice = input("Enter 1 or 2: ")
    
    if choice == '1':
        play_against_ai()
    elif choice == '2':
        play_against_player()
    else:
        print("Invalid choice. Please enter 1 or 2.")

# Run the game
main()
```

---

### Explanation of the Game Modes

1. **`play_against_ai()`**:
   - You take turns with the AI, which uses `minimax_solver` to determine its moves.
   - Each player (human and AI) chooses a pile and removes a number of matches, and the game displays the new state after each move.
   - The game ends when one pile is empty, and the winner is declared.

2. **`play_against_player()`**:
   - This mode allows two players to take turns without AI intervention.
   - Each player selects a pile and the number of matches to remove, and the game continues until one pile is empty.
   - The player who removes the last match is declared the winner.

3. **`main()`**:
   - This function allows the user to choose between playing against the AI or another player.

---

This setup provides a hands-on way to play Nim and test the minimax-based AI. You can use this version in your CS club to demonstrate both player vs. player and player vs. AI modes, and it’s especially useful for testing strategies discussed in your induction proof!
