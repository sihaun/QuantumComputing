# Pattern Searching Using Grover's Algorithm

## Overview

This project demonstrates how Grover's Algorithm can be applied to search for non-winning TicTacToe game states using quantum circuits. By representing a 3×3 TicTacToe board and checking for the absence of any "bingo" (three in a row), the algorithm helps identify patterns where the game has no winner.

## 1. TicTacToe Game

TicTacToe is a two-player game played on a 3×3 grid. Players take turns placing either "O" or "X", and the player who aligns three identical marks (horizontally, vertically, or diagonally) wins.

In this project, we aim to identify TicTacToe states where no player has won, using Grover’s quantum search algorithm.

## 2. Bingo Detector Circuit

To detect winning conditions ("bingo"), a logic circuit is implemented. The circuit activates (output `t = 1`) only if the input qubits represent either all "O" (`000`) or all "X" (`111`). The following truth table describes its logic:

| x₂ | x₁ | x₀ | t |
|----|----|----|---|
|  0 |  0 |  0 | 1 |
|  0 |  0 |  1 | 0 |
|  0 |  1 |  0 | 0 |
|  0 |  1 |  1 | 0 |
|  1 |  0 |  0 | 0 |
|  1 |  0 |  1 | 0 |
|  1 |  1 |  0 | 0 |
|  1 |  1 |  1 | 1 |

The circuit outputs 1 when it detects a bingo.

## 3. TicTacToe Circuit

To verify that there is no winning condition in the game, we must check all 8 possible bingos:
- **Horizontal (3):** Top, middle, bottom rows
- **Vertical (3):** Left, middle, right columns
- **Diagonal (2):** From top-left to bottom-right and top-right to bottom-left

Board indexing:

```
x₀ x₁ x₂
x₃ x₄ x₅
x₆ x₇ x₈
```

Checks:
- Horizontal: (x₀ x₁ x₂), (x₃ x₄ x₅), (x₆ x₇ x₈)
- Vertical: (x₀ x₃ x₆), (x₁ x₄ x₇), (x₂ x₅ x₈)
- Diagonal: (x₀ x₄ x₈), (x₂ x₄ x₆)

The circuit applies a NOT gate to the `y` qubit only when all 8 bingo conditions are not met, signifying a valid non-winning state.

## 4. Simulation

Before measurement, we initialize some positions as empty using the Hadamard gate (superposition). All uninitialized positions default to "O".

> ⚠️ Setting too many positions as empty can lead to computational complexity. Conversely, too few results in no valid solutions. It's recommended to set **6 to 7 empty positions**.

Example configuration:
- "O" at positions 0 and 4
- Empty positions at 1, 2, 3, 5, 6, 7, 8  
- Total possible solutions (n = number of empty spots): 7  
- Number of Grover iterations: 3 (calculated based on n)

## 5. Results

Measurement results showed distinct peaks corresponding to valid solutions.

The 7 valid solutions did not occur with perfectly equal probabilities (ideal would be ≈ 0.142 each), but their combined probability totaled approximately **0.991**, very close to 1.

### Non-Bingo Map Visualization

The visualized 3×3 maps for measured solutions confirmed that **no bingo condition was met** in any direction, verifying the effectiveness of the pattern search.

## 6. Conclusion

This project demonstrated pattern searching in a quantum setting using Grover’s algorithm, specifically applied to the TicTacToe game's non-winning states. The same method can be extended to other pattern recognition problems using similar quantum logic circuits.
