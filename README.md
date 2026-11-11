# tiny-chess-01

A chess engine in 4,096 bytes of Python, written for the make.fun
competition "Build the strongest chess engine in 4 KB of Python".

`main.py` is the whole engine: a single line that unpacks (lzma + base85)
and runs the engine. The readable source it was packed from is kept as
`engine_src.py` in the build directory (not part of this repository).

## How it plays

- 0x88 board, alpha-beta search with a transposition table
- Move ordering: transposition-table move, captures (MVV-LVA), killer
  moves, history heuristic
- Quiescence search over captures and promotions, and over every reply
  to a check; check extensions in the main search
- Evaluation: material plus formula piece-square tables with a
  phase-interpolated king table and a bishop-pair bonus; repetitions
  inside the search score as draws
- Deterministic: a fixed node budget per move (45,000 nodes, less in
  very long games), no clock, no unseeded randomness — the replay plays
  the same moves
- Talks to the env at `MAKEFUN_ENV_URL` (`POST /game`, `POST /move`),
  plays UCI moves, never resigns, and falls back to the first legal move
  if its search ever fails

## Size

`main.py` is exactly 4,096 bytes (the competition limit). The whole
repository (this README, the MIT LICENSE and `main.py`) is under the
8,192-byte tree cap.

## License

MIT — see LICENSE.
