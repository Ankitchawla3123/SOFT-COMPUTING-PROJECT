# Tetris GA

This repository implements a simple Genetic Algorithm (GA) to evolve a linear heuristic for playing Tetris.  
It includes the training process for learning weights and a greedy player that uses the trained weights.

---

## Files

### `tetris_ga_train.py`  (GA Create)
Runs the Genetic Algorithm to evolve a 4-weight vector for evaluating Tetris positions.

- Board size: 20×10  
- Pieces used: I, O, T, J, L (with all rotations)  
- Realistic spawn rule — a piece cannot spawn if blocked at the top  
- Feature vector (4-D):  
  1. Aggregate Height  
  2. Move Score  
  3. Holes  
  4. Bumpiness  
- Fitness metric: Average points per move,  
  \[
  F = \frac{S}{N_{\text{max}}}
  \]
- Saves per-generation logs to `ga_weights.csv`  
- Saves the best genome to `best_ga_weights.csv`

---

### `ga_weights.csv`  (GA Weights)
Contains the per-generation training log.

- Columns: `gen, fitness, w1, w2, w3, w4`  
- Each row represents one evaluated genome in that generation  
- Fitness is the average points per move achieved by that genome  

Example:
```
gen,fitness,w1,w2,w3,w4
0,0.628,-0.60079,0.17947,-0.69767,-0.34653
1,0.649,-0.60079,0.17947,-0.69767,-0.34653
...
```

---

### `best_ga_weights.csv`  (Best GA Weights)
Stores the best-performing genome after all generations are complete.

- Columns: `fitness_avg_points_per_move, w1, w2, w3, w4`  
- These are the final weights used for playing Tetris

Example:
```
fitness_avg_points_per_move,w1,w2,w3,w4
0.708,-0.59977,0.17723,-0.69841,-0.34795
```

---

### `tetris_ga_play.py`  (Play Tetris with GA)
A greedy Tetris player that uses the learned weights from `best_ga_weights.csv`.

- For each legal move, computes:
  \[
  \text{score} = w_1 \times \text{height} + w_2 \times \text{move\_score} + w_3 \times \text{holes} + w_4 \times \text{bumpiness}
  \]
- Chooses the move with the highest score  
- Prints:
  - Lines cleared per move  
  - Score gained per move  
  - Total lines and score  
  - Current board state  

---

## Quick Start

### 1. Train (Generate GA Weights)
```bash
python tetris_ga_train.py
```
Outputs:
- `ga_weights.csv` — Training progress  
- `best_ga_weights.csv` — Best evolved weights

---

### 2. Play (Use Trained Weights)
```bash
python tetris_ga_play.py
```
Requires `best_ga_weights.csv` in the same directory.

---

## Scoring System

NES-style scoring used per move:
| Lines Cleared | Points |
|---------------|---------|
| 0             | 0       |
| 1             | 2       |
| 2             | 5       |
| 3             | 15      |
| 4             | 60      |

Early game-overs are penalized since total score is divided by the fixed move budget \(N_{\text{max}}\).

---

## Output Files

- `ga_weights.csv` — Fitness progress across generations  
- `best_ga_weights.csv` — Final optimized weight vector  
- Console output during play shows move-by-move results and board states

---

## Reference

This implementation follows ideas inspired by:  
- [Lewis, J. (2015) — The Tetris Problem (CS229, Stanford)](https://cs229.stanford.edu/proj2015/238_report.pdf)  
- [Code My Road — Tetris AI: The Near Perfect Player](https://codemyroad.wordpress.com/2013/04/14/tetris-ai-the-near-perfect-player/)
