# tiny-chess-01 entry

A chess engine for the make.fun `tiny-chess-01` competition, in at most
4,096 bytes of Python 3.13 (standard library only).

## The engine

`main.py` is the whole entry: it talks to the env over HTTP
(`POST /game`, `POST /move`, `POST /resign`) and plays every game.

The search is a fail-soft alpha-beta with:

- a 128-square mailbox board and 64-bit Zobrist hashing,
- a tapered evaluation (PeSTO midgame and endgame piece-square tables,
  interpolated by the non-pawn material phase),
- iterative deepening with a fixed node budget, so the engine is
  deterministic: no clock, no unseeded randomness,
- a transposition table (depth, bound flag, score, move), null-move
  pruning, killer moves, MVV-LVA capture ordering and a quiescence search
  over captures, promotions and check evasions.

The node budget is cumulative across the iterative-deepening iterations and
is lowered when one side is far ahead on material, which keeps a whole trial
well inside the wall budget.

## Rules it follows

- `main.py` is at most 4,096 bytes as committed and reads no other file.
- Python 3.13 standard library only; no model calls.
- Deterministic: a fixed node count per move, never a clock.
- At most 10 s a move; the whole trial stays inside the 1,800 s wall budget.

## License

MIT, see `LICENSE`.
