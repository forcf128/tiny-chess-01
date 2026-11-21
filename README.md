# tiny-chess-01
[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/forcf128/tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/forcf128/tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
A 4 KB Python chess engine for make.fun tiny-chess-01: 0x88 board, alpha-beta
with transposition table, killer moves, late-move reductions and quiescence
search, under a fixed node budget per move. MIT licensed.

Engine v3f with fixed transposition-table flags and a promotion-capture UCI
fix; mean practice Elo 1,336 (150,000 nodes per move).

Packed entrypoint: 4,074 bytes (makefun compress, LZMA).

## Design
Board: 0x88 array of 128 squares, 16 per rank.
Pieces: 1 pawn, 2 knight, 3 bishop, 4 rook, 5 queen, 6 king.
White pieces are 1-6, black pieces are 9-14.
Move: 19 bits, from (7), to (7), promotion (3), flags (2).
Flags: bit 17 capture, bit 18 en passant, bit 19 castling.
Pawn moves: single push, double push, captures, promotions.
Eval: material plus piece-square tables, king tapered.
Search: alpha-beta with a transposition table.
Killer moves: two per ply, tried before quiet moves.
Late moves are searched shallow first, then full depth.
Quiescence covers captures and replies while in check.
Repetition and the fifty-move rule return a draw score.
Zobrist keys use a fixed seed, so play is deterministic.
Budget: fixed node count per move, no clock is read.
Node budget comes from the NODE_BUDGET environment variable.
Default budget is 100,000 nodes when the variable is absent.
The driver speaks the kit protocol over HTTP.
It applies the server moves, then searches and replies.
If the reply is illegal, it falls back to the first legal move.
The engine never calls a model and never uses randomness.
All randomness is seeded, so replays are byte-identical.
Piece values: pawn 100, knight 320, bishop 330, rook 500, queen 900.
Bishop pair is not scored separately in this version.
Pawn table rewards central and advanced pawns.
Rook table rewards the seventh rank.
King table keeps the king safe early and active late.
The transposition table holds up to 300,000 entries.
Practice: trial-0 1364, trial-1 1099, trial-2 1545.
Mean practice Elo 1336 at 150,000 nodes per move.
That is 29 Elo below the sunfish baseline of 1365.
Six games per trial is noisy, about 150 Elo per game.
The hybrid source keeps the setup readable and minifies the core.
Setup, driver and small helpers stay in readable form.
Search, move generation and make/unmake are minified.
Semantics are identical to v3f, so the Elo stands.
