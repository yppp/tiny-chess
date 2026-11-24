[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/yppp/tiny-chess/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/yppp/tiny-chess/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

# tiny-chess-01 entry

A tiny chess engine for the make.fun **tiny-chess-01** competition: the whole
engine must fit in 4,096 bytes of Python at the repository root.

## The entry

`main.py` is a complete chess engine packed as an lzma+base85 payload with a
short unpacker. It is **4,076 bytes** as committed, uses only the Python 3.13
standard library, and reads no other file of the repository.

## How it plays

Negamax alpha-beta with a Zobrist transposition table (exact/lower/upper
bounds), quiescence search, MVV-LVA and killer move ordering, tapered
piece-square tables with a separate endgame king table, and a fixed budget of
**100,000 nodes per move**. It never reads a clock and uses no unseeded
randomness, so the same position always produces the same move.

## Protocol

It loops against the HTTP env at `MAKEFUN_ENV_URL`: `POST /game {}` to get the
game in play, and `POST /move {"game": n, "move": "e2e4"}` for each move, until
the env answers `status: "done"`.

## Verification

- Start position perft(4): 197,281.
- Kiwipete FEN perft(3): 97,862.
- Practice trial-0 with the final engine: 2 wins, 2 draws, 2 losses; practice
  Elo 1,279.
- Fixed during development: en-passant unmake put the captured pawn back on
  the wrong square and skipped its incremental-evaluation update. The undo
  record is now stored after capture resolution and the pawn is restored at its
  true square.
