# Gomoku
___
## Team Name: Large_Horse
#### Contributors: Bhon Bunnag, Malika Nurbekova, Yil Verdeja
___
## 1 Project Description and Goal
This project consists of developing and implementing a computer program that plays Gomoku. Also known as "five in-a-row", is a board game similar to tic-tac-toe but on a larger board. This project will exemplify the minimax algorithm and alpha-beta pruning.
***
### 1.1 Game Description
Gomoku is a two player game. The two players take turns putting markers on the board. One of the player uses **white** markers and the other uses **black** markers. 

The size of the board is a 15 x 15 cell board. Columns are marked with letters *A* to *O* from left to right and rows are numbered 1 to 15 from top to bottom.

A move is specified by the column letter and row number pair (e.g., F8, G3, etc)
The player who gets 5 markers in a row wins. If the board fills up before anyone can win, the game ends in a tie.
***
### 1.2 Game Rules
A game will consist of a sequence of the following actions:
1. A random selection method is used to determine which player will use the white markers and which player will use the black markers. In what follows, the player who gets to use the white markers is called *player1* and the player who gets to use the black markers is called *player2*
2. *Player1* gets to play first.
3. After *Player1* has made its first move, *player2* is given the chance to change the color of the stone on the board to black. This is done to limit the advantages of playing first.
4. After that, *player1* and *player2* take turns making moves until the game ends with one player winning or a tie. There is a **10 second time limit** for a player to make its move.
___
## 2 Program Implementation
The following subsections will go through and describe the different algorithms, heuristics and strategies that were implemented to run the Gomoku AI. This section will also step through the AI's offensive and defensive behavior, as well as its interaction with the referee.

***
### 2.1 Minimax Implementation
The minimax implementation can be found within the *agent.py* class as a the function named *minimax*. The following snippets on lines __ to __ shows the minimax implementation

```python
def minimax(depth = 1):
    global white
    global black
    global bestMove
    global cutOff
    allValidMoves = getValidMoves()
    maxScore = -1<<31
    for move in allValidMoves:
        addMoveToBoard(move[0], move[1], True)
        curScore = alphaBeta(depth,alpha = -1<<31, beta = 1<<31, isMaxPlayer = False)
        if(curScore > maxScore):
            maxScore = curScore
            bestMove = move
        removeMoveFromBoard(move[0], move[1], True)
    return bestMove
```

***
### 2.2 Alpha-Beta Pruning Implementation
The alpha-beta pruning implementation can be found within the *agent.py* class as a the function named *alphaBeta*. The following snippets on lines __ to __ shows the minimax implementation

```python
def alphaBeta(depth = 3, alpha = -1<<31, beta = 1<<31, isMaxPlayer = False):
    global white
    global black
    validMoves = getValidMoves()
    levelScore = white.getScore()-black.getScore()
    if(depth == 1 or (abs(levelScore)>WIN_SCORE_CUTOFF)):
        return levelScore
    if(isMaxPlayer):
        maxScore = -1<<31
        for move in validMoves:
            addMoveToBoard(move[0], move[1], True)
            maxScore = max(maxScore, alphaBeta(depth-1,alpha, beta, False))
            alpha = max(alpha, maxScore)
            removeMoveFromBoard(move[0], move[1], True)
            if(beta <= alpha):
                break;
        return maxScore
    else:
        minScore = 1 <<31
        for move in validMoves:
            addMoveToBoard(move[0], move[1], False)
            minScore = min(minScore, alphaBeta(depth-1,alpha, beta, True))
            beta = min(beta, minScore)
            removeMoveFromBoard(move[0], move[1], False)
            if(beta <= alpha):
                break;
        return minScore
```

***
### 2.3 Heuristic Evaluation Function and Strategies

```python
def getScore(self):
        #Purely offensive strategy
        totalScore = 0
        alreadyExistingHeadTails = set()
        for move in self.orderedMoves:
            if(self.board[move[0]][move[1]]==1):
                boundaryList = [(1, 0),(0, 1),(1, 1),(-1, 1)]
                for vector in boundaryList:
                    head = (move[0] + vector[0], move[1] + vector[1])
                    tail = (move[0] - vector[0], move[1] - vector[1])
                    curLen = 1
                    ...
                    # Expands the Tail node and Head node until it reaches a different marker or an empty cell
                    ...
                    headTail = (head,tail)
                    if(headTail not in alreadyExistingHeadTails):
                        alreadyExistingHeadTails.add(headTail)
                        if(curLen >= 5): return self.score[5]
                        if((headBlock and not tailBlock) or (not headBlock and tailBlock)):
                            if(self.ourMove and curLen == 4): #A forced win
                                totalScore += 10000
                            else:
                                totalScore += self.score[curLen]
                        if(not (headBlock or tailBlock)):
                            if(curLen == 4): totalScore += 1000
                            totalScore += 2*self.score[curLen]
        return totalScore
```

```python
def depthLimited():
    minimax(2)
```

***
### 2.4 Offensive and Defensive Behavior

***
### 2.5 Interaction with Referee

***
### 2.6 Program Testing
The *GomokuCollection* class is the class that controls the movement on the board and assigns evaluation scores to it. In order to proceed in using this class, it needed to be heavily tested. Using Test Driven Development (TDD), inside the *testCollection.py* class, the GomokuCollection was thouroughly examined to make sure that the code worked as expected.

Initially to test the program, two different agents needed to be executed with the referee. As that became a slow and painstaking effort, the *gameTest.py* class was created in order to quicken this process. This was possible by changing the attributes of the team names and the agent files. An example is the snippet below which uses two example agents with their corresponding team names.

```python
def callReferee(teamName1 = 'Large_Horse', teamName2 = 'notKnuckles'):
    subprocess.call(['python.exe', 'referee.py', teamName1, teamName2])
def callAgent():
    subprocess.call(['python.exe', 'agent.py'])
def callAgent2():
    subprocess.call(['python.exe', 'agent2.py'])
def removeFiles(teamName1 = 'Large_Horse', teamName2 = 'notKnuckles'):
    listOfFiles = [teamName1 + ".go", teamName2 + ".go", "move_file",
                   "history_file", "end_game"]
    for e in listOfFiles:
        try:
            os.remove(os.path.join('./', e))
        except:
            print("No file named " + e)
```
___
## 3 Problems Faced
In this section, all the bugs that have been encountered during the process of programming the Gomoku AI are listed below as well as how they were fixed.
***
### 3.1 Bugs

***
### 3.2 Fixes

___
