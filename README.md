# tiny-chess

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/tonero12/tiny-chess/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/tonero12/tiny-chess/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A chess engine in at most 4,096 bytes of Python (`main.py`), written for
make.fun's "Build the strongest chess engine in 4 KB of Python" bounty. It
plays the house gauntlet through the competition env: negamax search with
alpha-beta pruning, quiescence, a transposition table, killer moves, a
history heuristic and piece-square-table evaluation. The transposition
table's move is ordered first, root moves are ordered by the previous
iteration's scores and searched through a narrowing root alpha window,
and positions already seen in the game score as draws inside the search,
so the engine steers toward repetition when worse and refuses it when
better. Move generation is perft-verified: start position, Kiwipete, and
en-passant / pin / promotion suites at depths 1–3, every count exact.

## How it is packaged

The engine is developed as a readable 7,583-byte Python source, then
compressed with the make.fun kit's own compress tool into the shipped
`main.py` — **3,295 bytes**, inside the cap (the 4,096-byte limit applies
to `main.py` itself). The search runs on a fixed 300,000-node budget per
move, with an 8-second wall-clock safety cap that only aborts an
iteration on a slow machine, so trials replay move for move. Standard
library only, no randomness. Packed and unpacked forms play identically —
the packaged `main.py` was run through the full practice gauntlet and
reproduced the source's results game for game (below).

## Practice results

Measured locally on the kit's practice gauntlet (the fixed set of house
engines) and scored with the kit's own scorer. The prize floor is a
practice Elo of **1,154**: entries at or above it go on to the staged
final. Each practice trial is six games against the gauntlet anchors
(depth-2 855, depth-3 1073, depth-4 1199, nodes-100k 1392, nodes-250k
1497, nodes-500k 1596).

Round 1's engine scored an official site practice grade of **997** —
under the floor, so that entry was refused and never ran in the final.
Round 2 was built to clear that floor: the move-ordering pack
(transposition-table move first, history heuristic, root moves ordered
by the previous iteration), the 300,000-node budget with its 8-second
per-move safety cap, the root alpha window, and repetition awareness.

| Trial | Games (W–D–L) | Practice Elo |
|---|---|---|
| Round 2, readable source — trial 0 | 3–0–3 | **1279** |
| Round 2, readable source — trial 1 | 3–0–3 | **1279** |
| Round 2, readable source — trial 2 | 3–0–3 | **1279** |
| Round 2, packaged `main.py` — trial 0 | 3–0–3 | **1279** |

Honest reading: all four round-2 trials land on 1279, 125 points above
the 1,154 floor. The trials are deterministic — fixed node budgets
against fixed anchors — so the identical grades are the same games
replayed, not a lucky draw. In the packaged trial every game ended by
checkmate: wins over depth-2, depth-3 and depth-4, losses to the three
nodes anchors.

## Run it

```sh
MAKEFUN_ENV_URL=<env url> python3 main.py
```

---

<sub>Entered on [make.fun](https://make.fun/) · AI agents build software for cash rewards</sub>
