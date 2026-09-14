# Good Words Solver Skill

A Claude skill that solves **Good Words** boards — the 5x5 Boggle-style word game with a 3-minute timer and a BONUS definition clue each round.

## What it does

Upload a screenshot of the board and Claude will:

1. Read the grid row by row.
2. Run `scripts/solve.py`, which walks every adjacent path (including diagonals, no tile reused) and checks against a large English word list plus generated inflections (plurals, -ED, -ER, -EST, -ING).
3. Reply with the BONUS word (if found) followed by every word of 5+ letters, longest first. 4-letter words on request.

## Game rules encoded

- Letters must touch, including diagonally; each tile used once per word.
- Minimum 4 letters. `Qu` counts as both letters.
- Scoring: 4→1, 5→2, 6→4, 7→8, 8→15, 9→25, 10→40, 11+→60. BONUS word is worth double.
- Obscure words and inflected forms are accepted by the game, so the solver output is **not** filtered.

## Files

```
good-words/
├── SKILL.md          # instructions Claude follows when the skill triggers
├── README.md
└── scripts/
    └── solve.py      # board solver
```

## Running the solver manually

```bash
pip install english-words --break-system-packages
python3 scripts/solve.py "N N A A I/A E O N I/S C O A P/O H O T D/T Y U J I"
```

Rows are separated by `/`, letters by spaces. Write the Qu tile as `Qu`. Output is `length word`, longest first.

## Improving it

If the game's "Words you missed" list contains words the solver didn't produce, add the missing pattern (usually an inflection rule or a dictionary source) to `scripts/solve.py`.
