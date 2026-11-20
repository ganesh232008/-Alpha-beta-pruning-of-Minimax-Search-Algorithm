# ExpNo 7 : Implement Alpha-beta pruning of Minimax Search Algorithm for a Simple TIC-TAC-TOE game
# Name: L Ganesh Kanna
# Register Number: 212225230072
# Aim:
Implement Alpha-beta pruning of Minimax Search Algorithm for a Simple TIC-TAC-TOE game

# GOALS of Alpha-Beta Pruning in MiniMax Search Algorithm
Improve the decision-making efficiency of the computer player by reducing the number of evaluated nodes in the game tree.
Tic-Tac-Toe game implementation incorporating the Alpha-Beta pruning and the Minimax algorithm with Python Code.
# IMPLEMENTATION
The project involves developing a Tic-Tac-Toe game implementation incorporating the Alpha-Beta pruning with the Minimax algorithm. Using this algorithm, the computer player analyzes the game state, evaluates possible moves, and selects the optimal action based on the anticipated outcomes.

# The Minimax algorithm
recursively evaluates all possible moves and their potential outcomes, creating a game tree.

# Alpha-Beta pruning
Alpha–Beta (𝛼−𝛽) algorithm is actually an improved minimax using a heuristic. It stops evaluating a move when it makes sure that it’s worse than a previously examined move. Such moves need not to be evaluated further.

When added to a simple minimax algorithm, it gives the same output but cuts off certain branches that can’t possibly affect the final decision — dramatically improving the performance

# Sample Input and Output:
<img width="778" height="687" alt="image" src="https://github.com/user-attachments/assets/6fb8c40a-e7f8-43c9-ae25-745f943d2b2e" />
<img width="782" height="526" alt="image" src="https://github.com/user-attachments/assets/6951b5f7-17d9-425c-82aa-d320c4f7d956" />
<img width="781" height="517" alt="image" src="https://github.com/user-attachments/assets/41466397-2cfb-43a0-b9f5-195fb68c8d73" />
<img width="778" height="520" alt="image" src="https://github.com/user-attachments/assets/7c01a941-988d-482e-9773-1633f15edddf" />
<img width="789" height="421" alt="image" src="https://github.com/user-attachments/assets/db664443-46b4-40c4-9036-1f23d0110cb4" />


# PROGRAM:
```
def initialize_game(self):
    self.current_state = [['.','.','.'],
                          ['.','.','.'],
                          ['.','.','.']]

    # Player X always plays first
    self.player_turn = 'X'

def draw_board(self):
    for i in range(0, 3):
        for j in range(0, 3):
            print('{}|'.format(self.current_state[i][j]), end=" ")
        print()
    print()
def is_valid(self, px, py):
    if px < 0 or px > 2 or py < 0 or py > 2:
        return False
    elif self.current_state[px][py] != '.':
        return False
    else:
        return True
def is_end(self):
# Vertical win
    for i in range(0, 3):
        if (self.current_state[0][i] != '.' and
            self.current_state[0][i] == self.current_state[1][i] and
            self.current_state[1][i] == self.current_state[2][i]):
            return self.current_state[0][i]

    # Horizontal win
    for i in range(0, 3):
        if (self.current_state[i] == ['X', 'X', 'X']):
            return 'X'
        elif (self.current_state[i] == ['O', 'O', 'O']):
            return 'O'

# Main diagonal win
    if (self.current_state[0][0] != '.' and
        self.current_state[0][0] == self.current_state[1][1] and
        self.current_state[0][0] == self.current_state[2][2]):
        return self.current_state[0][0]

# Second diagonal win
    if (self.current_state[0][2] != '.' and
        self.current_state[0][2] == self.current_state[1][1] and
        self.current_state[0][2] == self.current_state[2][0]):
        return self.current_state[0][2]

# Is the whole board full?
    for i in range(0, 3):
        for j in range(0, 3):
            if self.current_state[i][j] == '.':
                return None
# It's a tie!
    return '.'
def max_alpha_beta(self, alpha, beta):
    maxv = -2
    px = None
    py = None

    result = self.is_end()

    if result == 'X':
        return (-1, 0, 0)
    elif result == 'O':
        return (1, 0, 0)
    elif result == '.':
        return (0, 0, 0)

    for i in range(0, 3):
        for j in range(0, 3):
            if self.current_state[i][j] == '.':
                self.current_state[i][j] = 'O'
                (m, min_i, in_j) = self.min_alpha_beta(alpha, beta)
                if m > maxv:
                    maxv = m
                    px = i
                    py = j
                self.current_state[i][j] = '.'

                # Next two ifs in Max and Min are the only difference between regular algorithm and minimax
                if maxv >= beta:
                    return (maxv, px, py)

                if maxv > alpha:
                    alpha = maxv

    return (maxv, px, py)

def min_alpha_beta(self, alpha, beta):

    minv = 2

    qx = None
    qy = None

    result = self.is_end()

    if result == 'X':
        return (-1, 0, 0)
    elif result == 'O':
        return (1, 0, 0)
    elif result == '.':
        return (0, 0, 0)

    for i in range(0, 3):
        for j in range(0, 3):
            if self.current_state[i][j] == '.':
                self.current_state[i][j] = 'X'
                (m, max_i, max_j) = self.max_alpha_beta(alpha, beta)
                if m < minv:
                    minv = m
                    qx = i
                    qy = j
                self.current_state[i][j] = '.'

                if minv <= alpha:
                    return (minv, qx, qy)

                if minv < beta:
                    beta = minv

    return (minv, qx, qy)
def play_alpha_beta(self):
    while True:
        self.draw_board()
        self.result = self.is_end()

        if self.result != None:
            if self.result == 'X':
                print('The winner is X!')
            elif self.result == 'O':
                print('The winner is O!')
            elif self.result == '.':
                print("It's a tie!")


            self.initialize_game()
            return

        if self.player_turn == 'X':

            while True:
                start = time.time()
                (m, qx, qy) = self.min_alpha_beta(-2, 2)
                end = time.time()
                print('Evaluation time: {}s'.format(round(end - start, 7)))
                print('Recommended move: X = {}, Y = {}'.format(qx, qy))

                px = int(input('Insert the X coordinate: '))
                py = int(input('Insert the Y coordinate: '))

                qx = px
                qy = py

                if self.is_valid(px, py):
                    self.current_state[px][py] = 'X'
                    self.player_turn = 'O'
                    break
                else:
                    print('The move is not valid! Try again.')

        else:
            (m, px, py) = self.max_alpha_beta(-2, 2)
            self.current_state[px][py] = 'O'
            self.player_turn = 'X'
```
# OUTPUT:
<img width="767" height="760" alt="image" src="https://github.com/user-attachments/assets/ab4b54d6-1f02-4da0-b8a4-00fbaa1eba69" />

# RESULT:
Thus, the program was executed successfully.
