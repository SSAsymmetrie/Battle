import random

board = []
diff = 0
size = 0
bd_size = 0


print("Let's play BattleShip!")

 #build the board
def make_board(board):
    for x in range(0,bd_size):
        board.append(["O"]*bd_size)

 #set board type as difficulty
def size_diff():
     #this loop will only continue if a number is entered
     while True:
         try:
             size = int(raw_input("Level 1 or 2:"))
             break
         except ValueError:
             print "Invalid Entry, Please try Again."
     if (size == 1):
             bd_size = 5
             return bd_size
     elif(size == 2):
             bd_size = 10
             return bd_size

 bd_size = size_diff()

make_board(board)

def print_board(board):
    for row in board:
        print " ".join(row)


 print_board(board)

def random_row(board):
    return random.randint(0,len(board)-1)

def random_col(board):
    return random.randint(0,len(board[0])-1)

ship_row = random_row(board)
ship_col = random_col(board)

 #print ship_row
 #print ship_col

 #Everything from here on should go in your for loop!
 #Be sure to indent!
for turn in range(5):
    while True:
        try:
            guess_row = int(raw_input("Guess Row:"))
             break
             except ValueError:
                 print "Invalid Entry, Please Try Again."
    while True:
         try:
             guess_col = int(raw_input("Guess Col:"))
             break
         except ValueError:
             print "Invalid Entry, Please Try Again."
    if guess_row == ship_row and guess_col == ship_col:
       print "Congratulations! You sunk my battleship!"
       break
    else:
        if (guess_row < 0 or guess_row > 4) or (guess_col < 0 or guess_col > 4):
            print "Oops, that's not even in the ocean."
        elif(board[guess_row][guess_col] == "X"):
            print "You guessed that one already."
        else:
            print "You missed my battleship!"
            board[guess_row][guess_col] = "X"

     if turn == 4:
         print "Game Over"
         board[ship_row][ship_col] = "#"
     # Print (turn + 1) here!
     print turn + 1
     print_board(board)
     
     
     OTHER ugjkgkbkbkn
     
     
""" A somewhat odd and primitive game of battleship that expands upon the battleship game as demonstrated in
    Codeacademy's Python lesson 19/19
"""


from random import randint

board_large = []
board_small = []
player_one = {
    "name": "Player 1",
    "wins": 0,
    "lose": 0
}
player_two = {
    "name": "Player 2",
    "wins": 0,
    "lose": 0
}
total_turns = 0
win_state_change = 0


def build_board_large(board):
    """create a 5 x 5 board"""
    for item in range(5):
        board.append(["O"] * 5)


def build_board_small(board):   # create a 3 x 3 board
    """create a 3 x 3 board"""
    for item in range(5):
        board.append(["O"] * 5)


def show_board(board_one, board_two):
    """make the lists look more like a simple battleship graph"""
    print "Board One"
    for row in board_one:
        print " ".join(row)
    print "Board Two"
    for row in board_two:
        print " ".join(row)


def load_game(board_one, board_two):
    """each time the players decides to play again, start again from a fresh slate"""
    print "Let's play Battleship!"
    print "Turn 1"
    del board_one[:]
    del board_two[:]
    build_board_large(board_one)
    build_board_small(board_two)
    show_board(board_one, board_two)
    ship_col_large = (lambda x: randint(1, len(x)))(board_one)
    ship_row_large = (lambda x: randint(1, len(x[0])))(board_one)
    ship_col_small = (lambda x: randint(1, len(x)))(board_two)
    ship_row_small = (lambda x: randint(1, len(x[0])))(board_two)
    print "Board 1 ship column: " + str(ship_row_large)
    print "Board 1 ship row: " + str(ship_col_large)
    print "Board 2 ship column: " + str(ship_row_small)
    print "Board 2 ship row: " + str(ship_col_small)
    return {
        'ship_col_large': ship_col_large,
        'ship_row_large': ship_row_large,
        'ship_col_small': ship_col_small,
        'ship_row_small': ship_row_small
    }

ship_points = load_game(board_large, board_small)  # assign the new ship locations to the new dictionary


def player_turns():
    """alternate between player turns by checking for odd numbers"""
    if total_turns % 2 == 0:
        return player_two
    else:
        return player_one


def play_again():
    """if user wants to play again, restart / reload game"""
    global total_turns
    global ship_points
    answer = str(raw_input("Would you like to play again?"))
    if answer == "yes":
        total_turns = 0    # reset / start over from player one again
        show_board(board_large, board_small)
        ship_points = load_game(board_large, board_small)
    else:
        exit()


def best_out_of(win_state, player):
    """check the game statistics"""
    global total_turns
    if win_state == 1 and player["wins"] < 2:  # only do a check if player one the current game
        print "%s wins this game!" % (player["name"])
        play_again()
    elif win_state == 0 and total_turns == 6:
        print "This match was a draw"
        play_again()
    elif win_state != 0 and total_turns == 6:
        play_again()
    elif player["wins"] >= 2:     # check who won best out of 3
        print "%s wins best out of 3" % (player["name"])
        play_again()
    elif player["lose"] >= 2:
        print "%s lost best out of 3" % (player["name"])
        play_again()
    else:
        play_again()


def input_check(ship_row, ship_col, player, board):
    """check/handle the players guesses of the ship points"""
    global win_state_change
    guess_col = 0
    guess_row = 0
    while True:
        try:
            guess_row = int(raw_input("Guess Row:")) - 1
            guess_col = int(raw_input("Guess Col:")) - 1
        except ValueError:
            print "Enter a number only."
            continue
        else:
            break
    match = guess_row == ship_row - 1 and guess_col == ship_col - 1
    not_on_small_board = (guess_row < 0 or guess_row > 2) or (guess_col < 0 or guess_col > 2)
    not_on_large_board = (guess_row < 0 or guess_row > 4) or (guess_col < 0 or guess_col > 4)

    if match:
        win_state_change = 1  # notes that someone has won the current game
        player["wins"] += 1
        print "Congratulations! You sunk my battleship!"
        best_out_of(win_state_change, player)
    elif not match and player == player_two:  # check the current player to then correlate with the correct board size
        if not_on_small_board:
            print "Oops, that's not even in the ocean."
        elif board[guess_row][guess_col] == "X":
            print "You guessed that one already."
        else:
            print "You missed my battleship!"
            board[guess_row][guess_col] = "X"
        win_state_change = 0
        show_board(board_large, board_small)
    elif not match and player == player_one:
        if not_on_large_board:
            print "Oops, that's not even in the ocean."
        elif board[guess_row][guess_col] == "X":
            print "You guessed that one already."
        else:
            print "You missed my battleship!"
            board[guess_row][guess_col] = "X"
        win_state_change = 0
        show_board(board_large, board_small)
    else:
        return win_state_change

"""Start the game logic"""
for games in range(3):
    games += 1  # 3 games total
    for turns in range(6):  # 6 turns total = 3 turns for each player
        total_turns += 1
        if player_turns() == player_one:
            print "It's player Ones's turn"
            input_check(
                ship_points['ship_row_large'],
                ship_points['ship_col_large'],
                player_one, board_large
            )
        elif player_turns() == player_two:
            print "It's player Two's turn"
            input_check(
                ship_points['ship_row_small'],
                ship_points['ship_col_small'],
                player_two, board_small
            )
        else:
            break
        if total_turns == 6 and player_turns() == player_one:
            best_out_of(win_state_change, player_one)
        elif total_turns == 6 and player_turns() == player_two:
            best_out_of(win_state_change, player_two)
        else:
            continue
    if games == 3:
            print "The game has ended."
        #   print "Wins: %s" % (player_one[entry])
            exit()
    else:
        continue

Other skndsjnfjsffnjsnfjs
from random import randint

board = []
ship_locations = []

# "Magic" numbers
suggested_board_size = 5
suggested_number_of_ships = 2

def populate_board(size):
    """Generate the board of size x size, returns size"""
    if(size < 1): # A 1x1 game is trivial and a 0x0 game is unwinnable
        print "Invalid board size.  Let's try " + str(suggested_board_size) + "x" + str(suggested_board_size)
        size = suggested_board_size
    for i in range(size):
        board.append(["O"] * size)  # ["O"] for ocean, note it must be a list because strings are immutable!
    return size

def print_board(board):
    """Prints the board in human-readable form"""
    if len(board) > 0:
        for row in board:
            print " ".join(row)
    else:  # We should never see this error message.
        print "The board is empty."

def random_row(board):
    """Generates random row location for ship"""
    return randint(0, len(board) - 1)

def random_col(board):
    """Generates random row location for ship"""
    return randint(0, len(board[0]) -1)

def generate_hidden_ships(number):
    """Generates all of the hidden ships in the game"""
    for i in range(number):
        hidden_ship = [random_row(board), random_col(board)]
        ship_locations.append(hidden_ship)

def guess_valid_location(location):
    """Given a valid (on the board location), checks for hits, misses, or previous guesses"""
    if location in ship_locations:
        board[location[0]][location[1]] = "H"  # Mark the hit
        print_board(board)
        ship_locations.remove(location) # user wins when all ships are found (ship_locations is empty)
        print "HIT!"
    elif(board[location[0]][location[1]] != "O"):
        print "You already guessed that location!"
    else:
        board[location[0]][location[1]] = "X"
        print "MISS!"

def guess_location(board):
    """Asks for guesses, checks validity (on board vs not),
    if not valid, alerts user
    if valid, calls guess_valid_location to find hit or miss"""
    guess_row = int(raw_input("Guess the row: 0 to " + str(len(board)-1) + ". "))
    guess_col = int(raw_input("Guess the column: 0 to " + str(len(board[0])-1) + ". "))
    if((guess_row < 0 or guess_row > len(board)-1) or (guess_col < 0 or guess_col > len(board[0])-1)):
        print "That's not even in the ocean."
    else:
        guess_valid_location([guess_row, guess_col])
     
def determine_total_number_of_turns(board_size, num_of_ships):
    """Determines the total number of turns the player can have
can depend on size of board and number of ships, 
have not found good formula for this yet"""
    return num_of_ships + 2 

def game_play():
    """All of game play,
from choosing game size and number of battleships, to winning or losing"""
    game_over = False
    turns = 0

    print "Welcome to Battleship!"
    board_size = int(raw_input("Choose a size for the square board: "))
    board_size = populate_board(board_size) # make sure to not worry about invalid board size
    number_of_ships = int(raw_input("How many ships do you want to try to find? "))  

    if(number_of_ships < 1 or number_of_ships > board_size**2):
        print "Invalid number of ships. Let's try " + suggested_number_of_ships
        number_of_ships = suggested_number_of_ships

    generate_hidden_ships(number_of_ships)
    total_number_of_turns = determine_total_number_of_turns(board_size, number_of_ships)

    print "Here is the board.  Choose wisely! You have " + str(total_number_of_turns - turns) + " turns."
    print_board(board)
    while(not game_over):
        if (len(ship_locations) == 0):
            print "You won!" 
            game_over = True
        elif(turns == total_number_of_turns):
            print "You lost. Sorry, try again next time!"
            game_over = True
        else:
            guess_location(board)
            turns += 1
            print "You have " + str(total_number_of_turns - turns) + " turns left. Here's the board for you: "
            print 
            print_board(board)
            print 

game_play()



Other fnjdnfkdmfskdmfds

# Battleship IV. (four tiles big :)
# This while statement let's the game run forever.
# At the end of each game You have the option to change the variable "play", which results in breaking the while loop and finishing the game.
play ="y"
while play =='y':
    import random
    print "Welcome to Battlesip IV!"
    print
    # this is the multiplayer part option of the game
    # first i created a variable multi (it's value can be whatever except 1 or 2)
    # then i ask for users input to change this variable to 1 or two
    # while loop will only break if you enter the number in the list multi
    multi ="a"
    mp=['1','2']
    while multi not in mp:
        multi = raw_input("single (1) or multiplayer (2)? ")
    multi = int(multi)
    print   
    if multi == 2:
        player1 = raw_input('Player 1, please enter your name: ')
        print
        player2 = raw_input('Player 2, please enter your name: ')
        print

    # similar situation as with the multiplayer option, we change the variable "difficulty" to 1,2 or 3
    difficulty ='a'
    dif=['1','2','3']

    while  difficulty not in dif: 
        difficulty = raw_input('Choose your difficulty level 1-3 :')

    # converting difficulty into a integer value, so we can perform comparison
    difficulty =int(difficulty)
    print

    # here we define a function diff, that takes in as input the difficulty we have chosen
    # and returns the number of rows that should be used for that difficulty level.
    def diff(difficulty):
        if difficulty == 1:
            return 5
        elif difficulty == 2:
            return 8
        elif difficulty == 3:
            return 30

    # here we assign the variable skill the result of our diff function
    skill = diff(difficulty)

    # Building the board
    board = []

    # here we create the list that we will use as the upper list that numbers each column (1  2  3  4 ...)
    upper_list =[]
    for a in range(1,skill+1):
        upper_list.append(a)

    # the board will be modeled accoring to the difficulty/skill we have chosen
    for x in range(0,skill):
      board.append(["~ "] * skill)

    # if the number in the upper list gets 10 or higher, they use more space, so we only add one space between the numbers if they are 10+, otherwise we add 2 spaces. 
    def print_board(board):
        nl=''
        for i in upper_list:
            if i<10:
                nl = nl+str(i)+'  '
            else:nl = nl+str(i)+' '
        print nl
        # here we print the rows
        #basically after each list printed, we also print the row number
        p=0
        for row in board:
            p=p+1
            print " ".join(row),p
            print

    print_board(board)
    # Hiding the first part of the ship
    def random_row(board):
      return random.randint(0,len(board)-1)

    def random_col(board):
      return random.randint(0,len(board[0])-1)

    ship_row = random_row(board)+1
    ship_col_1 = random_col(board)+1

    # Creating the other parts of the ship, checking if they are next to each other and making sure that they stay on the board.

    def second_part(board):

        if ship_col_1 == len(board):
            return ship_col_1 - 1
        if ship_col_1 == 1:
            return ship_col_1 + 1
        else:
            return ship_col_1 + 1

    ship_col_2 = second_part(board)

    def third_part(board):
        if (ship_col_2 == len(board) or ship_col_1 == len(board)) and \
           (ship_col_2 == len(board)-1 or ship_col_1 == len(board)-1):
            return len(board)-2
        elif ship_col_1 == 1:
                return ship_col_1 +2
        elif (ship_col_2 == 0 or ship_col_1 == 0) and \
             (ship_col_2 == len(board)-1 or ship_col_1 == len(board)-1):
            return ship_col_2 -2
        else: return  ship_col_2 +1   

    ship_col_3 = third_part(board)

    def fourth_r(board):
        if ship_row == 1:
            return 2
        if ship_row == len(board):
            return len(board)-1
        else:
            return ship_row-1

    def fourth_c(board):
        return max(ship_col_1,ship_col_2,ship_col_3) # picking the highest value 

    ship_row_2 = fourth_r(board)
    fourth_col = fourth_c(board)
    tries = 6  # "tries" will track how many turns you have left
    while tries > 0:

      # these two ifs only execute if we have chosen the multiplayer option
      # player1 plays when the "tries" are a even number, otherwise it's player2 turn
      if tries%2 == 0 and multi ==2:
          print "It's Your turn",player1
          print
      if tries%2 != 0 and multi==2:
          print "It's Your turn",player2
          print
      guess_row = raw_input("Guess Row:")
      guess_col = raw_input("Guess Col:")

      if guess_row.isdigit() == False or guess_col.isdigit() == False:     # accepting only numeral entries. .isdigit() returns a True if a string contains a integer
          print 'Misfire ey?'

      elif (int(guess_row) == ship_row and int(guess_col) == ship_col_1)or \
           (int(guess_row) == ship_row and int(guess_col) == ship_col_2)or \
           (int(guess_row) == ship_row and int(guess_col) == ship_col_3) or \
           (int(guess_row) == ship_row_2 and int(guess_col) == fourth_col):  #  if either of the parts of the ship is hit, you won
        if (tries%2==0 or tries%2!=0) and multi ==1:
            print "Congratulations, You sunk my battleship!!"
        elif tries%2==0:
            print "Congratulations "+player1+"! You sunk my battleship!!"

        elif tries%2!=0:
            print "Congratulations "+player2+"! You sunk my battleship!!"

        guess_row = int(guess_row)-1
        guess_col = int(guess_col)-1
        # printing the ship in case of victory
        board[ship_row-1][ship_col_1-1] = "O "
        board[ship_row-1][ship_col_2-1] = "O "
        board[ship_row-1][ship_col_3-1] = "O "
        board[ship_row_2-1][fourth_col-1] = "O "
        board[guess_row][guess_col]='* '   # the part of the ship you hit, will be displayed with a *
        print_board(board)
        break
      else:
        guess_row = int(guess_row)-1
        guess_col = int(guess_col)-1
        if guess_row < 0 or guess_row > len(board) or guess_col < 0 or guess_col > len(board)-1:
          print "Oops, that's not even in the ocean."
        elif(board[guess_row][guess_col] == "X"):
          print "You guessed that one already."          
        else:
          print "You missed my battleship!"
          board[guess_row][guess_col] = "X "
          print_board(board)
          print "Number of tries you have left! "+ str(tries - 1)
          tries = tries - 1
          if  tries < 1:
              # printing the position of the ship, in case you loose
              board[ship_row-1][ship_col_1-1] = "O "
              board[ship_row-1][ship_col_2-1] = "O "
              board[ship_row-1][ship_col_3-1] = "O "
              board[ship_row_2-1][fourth_col-1] = "O "
              print_board(board)
              print "Game Over!!"
              print
              break
    # here we can choose to continue, or finishing the game by breaking the loop, with anything else then a 'y'  
    play = raw_input('Do you want to play again? press "y" for yes: ')

print "Thank You for playing Battleships IV !!"

     
