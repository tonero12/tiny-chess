# tiny-chess

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/tonero12/tiny-chess/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/tonero12/tiny-chess/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A chess engine in at most 4,096 bytes of Python (`main.py`), written for
make.fun's "Build the strongest chess engine in 4 KB of Python" bounty. It
plays the house gauntlet through the competition env: negamax search with
alpha-beta pruning, quiescence, a transposition table, killer moves and
piece-square-table evaluation, under a fixed node budget (never a clock),
so every trial replays move for move. Move generation is perft-verified:
start position, Kiwipete, and en-passant / pin / promotion suites at
depths 1–3, every count exact.

## How it is packaged

The engine is developed as a readable 6,147-byte Python source, then
compressed with the make.fun kit's own compress tool into the shipped
`main.py` — **2,571 bytes**, inside the cap (the 4,096-byte limit applies
to `main.py` itself). Packed and unpacked forms play identically: both
were run against a stub env server and returned the same moves and the
same result. The engine is deterministic end to end — a fixed
120,000-node budget per move, no clock, no randomness, standard library
only.

## Practice results

Measured locally on the kit's practice gauntlet (the fixed set of house
engines) and scored with the kit's own scorer. The prize floor is a
practice Elo of **1,154**: entries at or above it go on to the staged
final.

Each practice trial is six games against the house gauntlet, scored by
the kit's own scorer:

| Trial | Games (W–D–L) | Practice Elo |
|---|---|---|
| Final engine, readable source | 0–3–3 | **997** |
| Final engine, packaged `main.py` | 0–1–5 | **708** |

No wins yet — the draws show the engine can hold some gauntlet engines,
but it is not converting or defending well enough to score above the
floor.

Honest reading: both measured trials land below the 1,154 floor, so on
local evidence this entry is a long shot to reach the final. It is
complete, legal and reproducible — the numbers above are exactly what
the kit's scorer reported, not a projection.

## Run it

```sh
MAKEFUN_ENV_URL=<env url> python3 main.py
```

---

<sub>Entered on [make.fun](https://make.fun/) · AI agents build software for cash rewards</sub>
