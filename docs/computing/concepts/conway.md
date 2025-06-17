# Conway's game of life. Pg 102 - 106 of automate the boring stuff with python.
import random, time, copy
WIDTH = 60
HEIGHT = 20

# Create cells.
nextCells = []
for x in range(WIDTH):
    column = []
    for y in range(HEIGHT):
        if random.randint(0, 1) == 0:
            column.append('#') # Living
        else:
            column.append(' ') # Dead
    nextCells.append(column)

while True:
    print('\n\n\n\n\n')
    currentCells = copy.deepcopy(nextCells)
    
    for y in range(HEIGHT):
        for x in range(WIDTH):
            print(currentCells[x][y], end='')
        print()

    for x in range(WIDTH):
        for y in range(HEIGHT):
            leftCoord = (x - 1) % WIDTH
            rightCoord = (x + 1) % WIDTH
            aboveCoord = (y - 1) % HEIGHT
            belowCoord = (y + 1) % HEIGHT
    
            numNeighbours = 0
            if currentCells[leftCoord][aboveCoord] == '#':
                numNeighbours += 1
            if currentCells[x][aboveCoord] == '#':
                numNeighbours += 1
            if currentCells[rightCoord][aboveCoord] == '#':
                numNeighbours += 1
            if currentCells[leftCoord][y] == '#':
                numNeighbours += 1
            if currentCells[rightCoord][y] == '#':
                numNeighbours += 1
            if currentCells[leftCoord][belowCoord] == '#':
                numNeighbours += 1
            if currentCells[x][belowCoord] == '#':
                numNeighbours += 1
            if currentCells[rightCoord][belowCoord] == '#':
                numNeighbours += 1
    
            if currentCells[x][y] == '#' and (numNeighbours == 2 or numNeighbours == 3):
                nextCells[x][y] = '#'
            elif currentCells[x][y] == ' ' and numNeighbours == 3:
                nextCells[x][y] = '#'
            else:
                nextCells[x][y] = ' '
    time.sleep(1)
