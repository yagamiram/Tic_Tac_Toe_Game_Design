# Tic_Tac_Toe_Game_Design
This repo is about how to design a high level API for a two player Tic Tac Toe game..
========

#### Tic Tac Toe Design Problem ####

Design a higher level Tic-Tac-Toe game such that it can be used by a client program in future.

### Project Components ###

1. Game Controller - A Controller that control the entire game.
2. Player - An object representing a player.
3. State - Maintains the current state of the game.


### Project Description ###

The Game controller creates the instances of two players and draws the board as per the configuration.
It triggers the game and through the instances of state, each state transition is validated after each move
and the result are verified.

Sample video of the link:
![Class Diagram](http://i67.tinypic.com/208tzwx.jpg)

### Basic Template###

```
'''
Starting point of the Tic-Tac-Toe game
Class: GameController
Description: It creates the board and gets the player info 
and starts the game between them. Using other instances like
state and player it validates each move made by player
and returns the result at the end of the game. A free_space_counter
is used to have the count # of free cells in the matrix. 
If the counter becomes 0 then the game is ended as drawn.

GAMECONTROLLER is the Entry point of this API for client.
'''
game_state = ENUM("WON", "LOST", "DRAW", "PROGRESS")
symbols = ENUM("O", "X", "NONE")

class GameController(object):
    # Constructor which takes rows and columns
    def __init__(rows, columns):
        self.rows = rows
        self.columns = columns

    def start(self):
        '''
        Starting point of the game.
        It creates the object for State and Players
        The state and players information aren't stored in DB
        and they will be erased at the end of the game.
        '''
        self.state = State()
        self.player1 = Player()
        self.player1.getInfo()
        self.player2 = Player()
        self.player2.getInfo()
        self.play()

    def play(self):
        '''
        The Tic-Tac-Toe game starts here!
        A Player is chosen randomly at the start of the game
        '''
        import random
        current_player = random.shuffle([player1, player2])
        while True:
            if state.free_cells_counter == 0:
                # No more free space, hence the game is drawn
                Print "The game is drawn"
                self.end()
                self.exit(1)
            # Now the current player should specify the row,col to enter his symbol
            # Assume the player has entered the row and column. If this is used as an API
            # then the client should enter the row and column as input.
            # Now make_move function is called
            status = self.make_move(row, col, current_player.symbol)
            if staus == game_state.WON:
                # The game is over. The current player is the winner
                # Exit the game
                Print "The winner is", current_player.get_name()
                # Release the memory consumed for playing the game 
                # Exit after that.
                self.end()
                sys.exit(1)
            # Now pass over the chance to the next player
            if current_player == player1:
                current_player = player2
            else:
                current_player = player1
    
    def make_move(self, row, col, symbol):
        # Check if the given row and col are valid
        # If not ask the client to give a valid row and col
        while True:
            if self.state.isValidMove(row, col):
                # The move is a valid one
                # Update the state
                self.update_state(row, col, symbol)
                # Now check if this moves ends the game or not
                if self.state.checkIfWon(row, col):
                    # game ends
                    return game_state.WON
                return game_state.PROGRESS
            # Get the input from client
            row, col = self.get_input() # Dummy function as of now
    
    def end(self):
        del self.state
        del self.player1
        del self.player2
        gc.collect() # Garbage Collector
    
'''
This class maintains the state of the game
'''
class State:
    # constructor creates a board with the # rows and # col specified by the client
    def __init__(self, rows, columns):
        self.rows = rows
        self.columns = columns
        self._matrix = [[symbols.NONE for _ in self.columns] for _ in self.rows]
        self.free_cells_counter = self.rows * self.columns
    
    def isValidMove(self, row, col):
        # Check if the cell is already marked or not
        # It is assumed that the player will click on the board and not provide 
        # the rows and columns as numbers
        return self._matrix[row][col] == symbols.NONE

    def updateState(self, row, col, symbol):
        self._matrix[row][col] = symbol
        self.free_cells_counter -= 1
    
    def checkIfWon(row, col):
        # check if the particular row ,col , left & right diagonal has same values
        if len(set([value for value in matrix[row]])) == 1:
            # The rows has same symbols. 
            return True
        elif len(set([matrix[r][col] for r in self.rows])) == 1:
            # The column has same symbols
            return True
        elif row == col or abs(row + col) == self.rows :
            # Check if the rows and cols fall in diagonal
            # Assuming the game is played in a square matrix
            # Check left diagonal
            if len(set([matrix[r][r] for r in self.rows])) == 1:
                # The left diagonal has same symbols
                return True
            elif len(set([matrix[r][self.rows-r] for r in self.rows])) == 1:
                #  The right diagonal has same symbols
                return True
        return False

'''
Maintains the player's information
'''
class Player(object):
    def __init__(self):
        self.name = None
        self.id = None
        self.symbol = None
    def getinfo(self):
        # This function will get the information
        # from the client to fill the attributes
        # name, id and symbol using getter and setter function
        # NOTE: Same symbol is not locked for different players
        pass
```
