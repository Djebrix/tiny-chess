[![tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/Djebrix/tiny-chess/rank.svg)](https://make.fun/bounty/tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/Djebrix/tiny-chess/reward.svg)](https://make.fun/bounty/tiny-chess-01)

# tiny-chess

A chess engine in under 4 KB of Python, written for the
[make.fun tiny-chess-01 bounty](https://make.fun/bounty/tiny-chess-01).
`main.py` (3,768 bytes) unpacks an lzma payload at startup and plays
through the competition environment's HTTP interface.

## How it works

- **Board:** 0x88 representation (128 squares, off-board reads return 0),
  so move generation needs no edge checks.
- **Move generation:** full legal chess — castling, en passant, all four
  promotions, and check evasion. Verified by perft (startpos to depth 4:
  20 / 400 / 8,902 / 197,281; kiwipete, pos3, pos4, pos5 all exact).
- **Search:** negamax alpha-beta with quiescence (captures/promotions,
  all moves when in check), MVV-LVA ordering, delta pruning, iterative
  deepening to depth 8 with a fixed 200,000-node budget per move, a
  transposition table (incremental Zobrist hash) with best-move ordering,
  and killer-move ordering.
- **Evaluation:** material (P100 N320 B330 R500 Q900) plus piece-square
  tables, updated incrementally on every make/unmake.
- **Determinism:** fixed node budget, no clock, no unseeded randomness —
  replays match move for move.

## Files

- `main.py` — the engine (entrypoint, run as `python3 -P -s /bot/main.py`)
- `LICENSE` — MIT
