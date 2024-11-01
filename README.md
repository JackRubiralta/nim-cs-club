Certainly! Here’s the full Markdown file with the addition of the interactive **Play Against AI** and **Play Against Another Player** modes at the end.

---

# Nim Game Instructions and Coding Exercise

## Game Rules

In the parlor game **Nim**, there are two players and **two piles of matches**. The game rules are simple:
1. Players take turns, with each player removing some (non-zero) number of matches from only **one pile**.
2. The **player who removes the last match wins** the game.

### Objective
Your task is to:
1. Prove a winning strategy for the **second player** using **strong induction**.
2. Complete the **minimax function** to create a simple AI for Nim.

---

## Part 1: Proving a Winning Strategy Using Strong Induction

To solve Nim, you’ll need to prove that the **second player has a winning strategy** if both piles start with the same number of matches. Use **strong induction** to show that the second player can always win if they play optimally.

### Induction Proof Outline

1. **Base Case**:
   - Start by considering the smallest possible game (for example, 1 match in each pile) and determine the winning move for the second player.

2. **Inductive Hypothesis**:
   - Assume that the second player has a winning strategy for any game that starts with two piles of `n` matches each.
   
3. **Inductive Step**:
   - Prove that this strategy also works if each pile has `n + 1` matches.

Once you've completed this proof, you’ll be ready to implement the strategy in code!

---

## Part 2: Completing the Minimax Function for Nim

To implement this strategy as an AI, you’ll create a `minimax` function that evaluates possible moves and returns the best choice for the current player.

### Code Skeleton for `minimax`

Below is a partially completed version of the **Nim** class and the **minimax** function. Follow the comments to complete each step.

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

# Incomplete minimax function with comments only
def minimax(game, is_current_player_turn):
    # Base case: if the game is over, return -1 if it's the current player's turn (loss), +1 otherwise (win)
    if game.is_terminal():
        # Check if it's the current player's turn and return the appropriate score
        return 1 if is_current_player_turn else -1

    # Initialize the best score depending on the player's turn
    if is_current_player_turn:
        # For the maximizing player, start with a very low score
        best_score = float('-inf')
    else:
        # For the minimizing player, start with a very high score
        best_score = float('inf')

    # Loop through each possible move
    for move in game.get_possible_moves():
        # Step 1: Copy the game state (hint: use copy.deepcopy)
        
        # Step 2: Apply the move on the copied game state
        
        # Step 3: Call minimax on the new game state, but for the next player's turn
        # Remember to toggle is_current_player_turn to switch turns
        
        # Step 4: Update best_score based on whether it’s the maximizing or minimizing player's turn
        # Hint: If it's the maximizing player's turn, find the highest score.
        # If it's the minimizing player's turn, find the lowest score.

    return best_score

# Testing function to display evaluation of each move
def minimax_solver(game):
    # Find the best move for the current player (assuming it’s their turn)
    best_score = float('-inf')
    best_move = None
    print("Evaluating moves and scores:")
    for move in game.get_possible_moves():
        # Create a deep copy of the game state and apply the move
        new_game = copy.deepcopy(game)
        new_game.make_move(move)

        # Evaluate this move using minimax
        score = minimax(new_game, False)  # Assume opponent's turn next
        print(f"Move: {move}, Score: {score}")

        # Update the best score and best move
        if score > best_score:
            best_score = score
            best_move = move

    print(f"Best move: {best_move} with score: {best_score}")
    return best_move
```

### Instructions

1. **Fill in `minimax()`**:
   - Follow the comments in `minimax` to complete each step.
   - For **Step 1**, copy the game state using `copy.deepcopy()`.
   - For **Step 2**, apply the move on the copied game state with `new_game.make_move(move)`.
   - For **Step 3**, call `minimax` recursively for the next player’s turn by toggling `is_current_player_turn`.
   - For **Step 4**, update `best_score` by finding the highest score if it’s the maximizing player’s turn, or the lowest if it’s the minimizing player’s turn.

2. **Run the Tests**:
   - The tests below will help verify the correctness of your minimax implementation.
   - Run these tests to confirm that the AI behaves as expected.

```python
# Tests for minimax and nim game logic
def test_minimax():
    game2 = Nim(3, 3)
    assert minimax(game2, True) == 1, "Test Case 2 Failed"  # First player can win

# Run the tests
test_minimax()
```

---

## Part 3: Interactive Game Modes

Once you've completed the `minimax` function and verified its correctness with tests, try playing Nim in one of the following modes:

### Play Against the AI
In this mode, you’ll play against an AI that uses the minimax strategy to select moves.

```python
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
```

### Play Against Another Player
In this mode, you can play against a friend without AI intervention.

```python
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
```

### Main Menu to Choose Game Mode

Use this main function to choose between playing against the AI or another player.

```python
def main():
    print("Choose a game mode:")
    print("1.

 Play against AI")
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

This exercise will help you understand both **strong induction** and the **minimax algorithm** in game theory. Test your skills by playing Nim against the AI or a friend. Good luck and have fun!
