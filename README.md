# Longest Path in a Grid Maze

This project explores the longest simple path problem in a grid maze. The solver treats the maze as a grid graph and uses depth-first search with backtracking and pruning to search for the longest path through open cells without revisiting any cell.

The repository contains two ways to run the maze solver:

- A console solver that reads a maze from a text file and prints the best path.
- A Swing GUI that animates the search and highlights the current search path and the best path found so far.

## What The Solver Does

The solver starts from every open cell in the maze and explores all valid four-directional moves: up, down, left, and right. As it searches, it keeps track of:

- the current path being explored,
- the best path found so far,
- which cells have been visited,
- and a pruning limit that stops branches that can no longer beat the best result.

This is an exponential-time search problem in the general case, so the pruning step matters a lot for performance.

## Repository Layout

- [src/LongestPathMaze.java](src/LongestPathMaze.java) - console version of the solver.
- [src/LongestPathGUI.java](src/LongestPathGUI.java) - Swing visualization of the solver.
- [mazes/4x4.txt](mazes/4x4.txt) - sample maze input.
- [mazes/6x6.txt](mazes/6x6.txt) - larger sample maze input.
- [src/SnakeAI.java](src/SnakeAI.java) - a related visual maze-search experiment.
- [src/TimeConversion.java](src/TimeConversion.java) and [src/time.java](src/time.java) - unrelated utility programs that are also present in the repository.

## Maze File Format

Maze files are plain text. Each line is a row in the grid.

- `.` means an open cell.
- `#` means a wall.

All rows should be the same length. For example:

```text
....
.###
....
....
```

## How To Run

The project does not use a build tool such as Maven or Gradle. You can compile it directly with `javac`.

### 1. Open a terminal in the project root

Make sure your terminal is opened in the folder that contains `README.md`, `src`, and `mazes`.

### 2. Compile the Java files

Create an output directory and compile the sources into it:

```powershell
mkdir out
javac -d out src\*.java
```

If `out` already exists, you can skip the `mkdir` command.

### 3. Run the console solver

Pass one of the maze files as an argument:

```powershell
java -cp out LongestPathMaze mazes\4x4.txt
```

Example output includes the longest path length and a visualization of the maze with the path marked.

### 4. Run the GUI visualizer

Launch the animated version with:

```powershell
java -cp out LongestPathGUI
```

In the GUI, you can:

- choose from several built-in mazes,
- start or stop the solver,
- and adjust the animation speed.

## How The Code Works

### `LongestPathMaze`

This is the command-line implementation. It:

1. loads a maze from a text file,
2. counts the number of open cells,
3. starts a DFS search from every open cell,
4. backtracks whenever it reaches a dead end,
5. and prunes branches that cannot beat the current best path.

When the search finishes, it prints the best path length and a copy of the maze with the solution drawn on top.

### `LongestPathGUI`

This class wraps the same search idea in a Swing interface. It adds:

- a maze panel that draws walls, the current path, and the best path,
- labels that show the current search depth and best result,
- a thread so the UI stays responsive while the solver runs,
- and an animation delay so the backtracking process can be watched in real time.

The GUI uses a few built-in maze layouts instead of loading a file, which makes it easier to demo the algorithm visually.

## Algorithm Notes

The problem is NP-hard in general, so this project is a brute-force search with one important optimization: pruning.

The pruning check is based on the idea that if the current path length plus all remaining unvisited cells still cannot exceed the best path already found, that branch can be abandoned immediately.

In practice, this makes small and medium mazes much more manageable, but very open mazes can still take a long time.

## Tips

- Smaller mazes are much faster to solve.
- Mazes with corridors and bottlenecks usually prune better than wide-open grids.
- For the GUI, lower animation delays make the solver feel faster, while higher delays make the search easier to follow.

## Example Inputs

You can experiment with the files under `mazes/` or create your own maze text files using the same `.` and `#` format.

## Notes

- The solver uses only standard Java libraries.
- The GUI and console solver are in the default package, so compile and run them from the project root.

