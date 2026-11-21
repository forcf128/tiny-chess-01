# tiny-chess-01
A 4 KB Python chess engine for make.fun tiny-chess-01: 0x88 board, alpha-beta
with transposition table, killer moves, late-move reductions and quiescence
search, under a fixed 50,000-node budget per move. MIT licensed.

Engine v3f with fixed transposition-table flags and a promotion-capture UCI
fix; mean practice Elo 1,336 (150,000 nodes per move).

Packed entrypoint: 4,002 bytes (makefun compress, LZMA).
