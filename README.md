A-NXN-Puzzle-Solver
===================

A Exploratory Decomposition Methodology example:

1. Introduction:

In this project I have implemented an algorithm for solving various sizes of the “Mystic Square” puzzle.  The “Mystic Square” puzzle, also called an N-puzzle, consists of an n-by-n board, where N = n*n -1, with N tiles numbered from 1 to N arranged randomly on the board.  There is one empty slot on the board.  Moves are made by sliding tiles into the empty position, effectively swapping their location with the empty position.  Through a sequences of adjacent swaps, the goal is to arrange the tiles in numerical order 1 at the top left with 2, 3, etc to the right and then continuing in order across each row until finally the last tile N is at the bottom right of the board.  Depending on the rules, the empty spot’s final position is either after the N tile in the bottom right corner cell or it is before the 1 tile in the top left corner square.    In my implementation I have solved puzzles by placing the empty spot at the top left of the board.  Doing this gave me a nice correspondence to an array representation of the board where tile “x” belonged in position x in the array and the empty slot was mapped to position 0. 

2.My Approach:

My method for solving an N-Puzzle was to use a branch and bound search algorithm which explores the tree of possible successive moves looking for the goal state of the board.   The root node of the tree corresponds to the initial configuration of the board.  After checking to make sure the board is solvable, I began to explore moves and board states in a very systematic way.  The algorithm has two classifications of nodes.  There is the Active Node which corresponds to the board state that I am currently looking at and then there is a bucket of Alive Nodes which contain all the nodes that I have not examined yet for solutions.  If the Active Node is not a goal state then I generate all the possible children from making a single swap with the empty tile and add these children to the Alive Nodes.  The Alive Nodes are sorted according to a cost function and I examine the children from least cost to greatest cost.   I have used the C++ STL’s Priority Queue to keep them sorted in the proper order.   This form of searching is completely dependent on the assignment of cost to the nodes that I am exploring.  

One example of a cost function is the number of moves I’ve made to get to a given board state, this cost function alone entails a breadth first search  as board states are visited from least moves to greatest number of moves.  While a breadth first search is guaranteed to find the shortest number of moves to the goal state, a breadth first search has a high time and space requirement as I would need to keep up to 3^depth number of nodes in memory where depth is the number of moves to the solution.  An 80 move best solution requires close to 3^80 nodes to be visited and held in memory.   If I change my cost function to be an estimate of how far I am away from a solution then my searching turns into more of a depth first search as I always expand the path which seems to be getting closer to a goal state.   Due to the distance being an estimate when I do find a goal state I am not certain that it is the best solution.  In my algorithm I preserve the goal states that I find and then continue searching for a more optimal solution.   In order to make sure the program terminates, I’ve placed bounds on time and space.  My algorithm will return the best result found within 10 minutes of searching, or within 30 million allocated nodes in memory, or it will terminate with the best solution possible if no bound is reached.

3.What Works: 
	I have tested my program with 24-Puzzles (5x5 boards).  I am not finding the shortest path to the goal state because I run out of space in memory, hitting the 30 million active node boundary.  My algorithm does find several successive solutions, increasingly tightening the number of moves.  On 4x4 boards I believe my algorithm is finding the optimal solutions quickly but in order to prove the solution is the best, all possible shorter sequences of moves must be tried.  For 4x4 boards which require 80 or more moves to solve, my algorithm times out after 10 minutes of searching.   On 3x3 boards my algorithm terminates on all boards within a few seconds.   I have not tested my program on 6x6 boards and I am unable to handle a board that size without recompiling for my code and generating an executable dedicated to handling boards of that size or higher.  I am confident that my program will perform on 6x6 boards the same way it does on 5x5 boards, finding a solution but then timing out after the space boundary is reached.
	
4.Cost function definition:
I have tried 4 kinds of cost function among which I am using 3.

Cost Function 1: Number of moves so far
Weight: 3

Cost  Function 2:  Number of tiles out of place. 
Weight: 0
I did not find this cost function to be effective. 

Cost Function 3:  Manhattan distance of tiles from their required position.
Weight : 3
This is the sum of the row and column displacements for each tile.  

Cost Function 4: Ranked Manhattan Distance. 
Weight: 5
The rank of a tile is max(row, column) that the tile is supposed to be on.  This function multiplies the Manhattan distance by the rank of the tile and sums the results.   This effectively states it is more important for the tiles of higher rank (further from the 0 position) to be in their proper positions.  

Total Cost = 3 * numOfMoves + 3 * ManhattanDistance  + 5*RankedManhattanDistance

5.Implementation of branch and bound technique: 
I have a single active node which represents the node that I am currently working with.  If the active node is not a Goal state then I generate its children based on the possible board moves and add the children to the Alive bucket of nodes.  I prune children which undo the moves of their parent, eliminating 2-cycles before the child nodes are generated.  After generating children and the active node is not a goal state, I get a new active node from the Alive bucket and check to see if it is a goal if so then I compare the current solution to the upper bound which stores the old solution.  If the new active nodes is not a goal, I check for cycles and if it is a cycle then I recycle the node to free up memory.  My solution also frees up any parents which no longer have children which are in memory.    The Alive bucket of nodes is implemented as an STL priority queue and always returns the node of least cost.   If the active node is not a solution I also check to make sure that the Manhattan distance from the goal  + moves so far is less than the upper bound’s total number of moves.  I can prune any nodes which we know will take longer to reach goal than the current upper bound.   If at any point my space and time bounds are hit then I terminate the search and return the current upper bound as the best solution so far. 

6.Updating upper bound: 
In my program the upper bound is a reference to the goal node of fist solution which has the information of number of moves to get the first solution. Initially upper bound is set to NULL. When fist solution is found, the upper bound is pointed to the goal node. When a better solution is found the upper bound is updated to point the goal node of this solution which has the new number of moves to reach goal state.  A new found goal replaces the upper bound when its moves are less than the number of moves in the upper bound.  



