# tic-tac-toe
tic tac toe game powered by AI
board = [' ' for x in range(10)]

def printGuideBoard():
    print('Please use numbers in the keyboard with this guide to play')
    print('   |   |')
    print(' ' + '1' + ' | ' + '2' + ' | ' + '3')
    print('   |   |')
    print('-----------')
    print('   |   |')
    print(' ' + '4' + ' | ' + '5' + ' | ' + '6')
    print('   |   |')
    print('-----------')
    print('   |   |')
    print(' ' + '7' + ' | ' + '8' + ' | ' + '9')
    print('   |   |')
    print('-----------------------LETS PLAY-----------------------')


def printBoard():
    print('   |   |')
    print(' ' + board[1] + ' | ' + board[2] + ' | ' + board[3])
    print('   |   |')
    print('-----------')
    print('   |   |')
    print(' ' + board[4] + ' | ' + board[5] + ' | ' + board[6])
    print('   |   |')
    print('-----------')
    print('   |   |')
    print(' ' + board[7] + ' | ' + board[8] + ' | ' + board[9])
    print('   |   |')


def spaceIsFree(pos):
    return board[pos] == ' '


def isBoardFull(board):
    if board.count(' ') > 1:  
        return False
    else:
        return True


def insertLetter(letter, pos):
    board[pos] = letter
    
    
def isWinner(bo, letter):
    return ((bo[7] == letter and bo[8] == letter and bo[9] == letter) or            # across the top
            (bo[4] == letter and bo[5] == letter and bo[6] == letter) or            # across the middle
            (bo[1] == letter and bo[2] == letter and bo[3] == letter) or            # across the bottom
            (bo[7] == letter and bo[4] == letter and bo[1] == letter) or            # down the left side
            (bo[8] == letter and bo[5] == letter and bo[2] == letter) or            # down the middle
            (bo[9] == letter and bo[6] == letter and bo[3] == letter) or            # down the right side
            (bo[7] == letter and bo[5] == letter and bo[3] == letter) or            # diagonal
            (bo[9] == letter and bo[5] == letter and bo[1] == letter))              # diagonal

def playerMove(symbol):
    vaildAns = True
    while vaildAns:
        move = input('Please select a position to place an ' + '(' +symbol + ')' + ' (1-9): ')
        try:
            move = int(move)
            if move > 0 and move < 10:
                if spaceIsFree(move):
                    vaildAns = False
                    insertLetter(symbol, move)
                else:
                    print('Sorry, this space is occupied!')
            else:
                print('Please type a number within the range!')
        except:
            print('Please type a number!')
          
                  
def selectRandom(li):
    import random
    ln = len(li)
    r = random.randrange(0, ln)
    return li[r]


def compMove():
    possibleMoves = [x for x, letter in enumerate(board) if letter == ' ' and x != 0]
    move = 0
    for let in ['O','X']:
        for i in possibleMoves:
            boardCopy = board[:]
            boardCopy[i] = let
            if isWinner(boardCopy, let):
                move = i
                return move
            
    cornersOpen = []
    for i in possibleMoves:
        if i in [1,3,7,9]:
            cornersOpen.append(i)
    if len(cornersOpen) > 0:
        move = selectRandom(cornersOpen)
        return move
    
    if 5 in possibleMoves:
        move = 5
        return move

    edgesOpen = []
    for i in possibleMoves:
        if i in [2,4,6,8]:
            edgesOpen.append(i)
    if len(edgesOpen) > 0:
        move = selectRandom(edgesOpen)

    return move


def playerVsPlayer():
    printGuideBoard()
    printBoard()
    while not(isBoardFull(board)):
        if not(isWinner(board, 'O')):
            if isBoardFull(board):
                print('Tie Game!')
                break
            playerMove('X')
            printBoard()
        else:
            print('Player Two (O) WON the Game')
            break
        
        if not(isWinner(board, 'X')):
            if isBoardFull(board):
                print('Tie Game!')
                break
                
            playerMove('O')
            printBoard()    
        else:
            print('Player One (X) WON the Game')
            break

def playerVsComputer():
    printGuideBoard()
    printBoard()
    while not(isBoardFull(board)):
        if not(isWinner(board, 'O')):
            playerMove('X')
            printBoard()
        else:
            print('Sorry, The Computer(O) won the Game!')
            break

        if not(isWinner(board, 'X')):
            move = compMove()
            if move == 0:
                print('Game is a Tie! No more spaces left to move.')
            else:
                insertLetter('O', move)
                print('Computer placed an \'O\' in position', move, ':')
                printBoard()
        else:
            print('X\'s win, good job!')
            break

    if isBoardFull(board):
        print('Tie Game!')


def main():
    print('Welcome to Our Tic Tac Toe Game!')
    print('----------------------------------------')
    print('1. Player vs Player')
    print('2. Player vs Computer')
    select = int(input('Please Choose what do you want: '))
    
    if select == 1:
        playerVsPlayer()
    elif select == 2:
        playerVsComputer()
    else: 
        print('Invaild Choose!')    

    
main()

while True:
    answer = input('Do you want to play again? (Y/N): ')
    if answer.lower() == 'y' or answer.lower == 'yes':
        board = [' ' for x in range(10)]
        print('-----------------------------------')
        main()
    elif answer.lower() == 'n' or answer.lower == 'no':
        break
    else:
        print('Invaild Choose!')
        continue
