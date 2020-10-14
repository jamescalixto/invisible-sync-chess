# invisible-sync-chess

blind synchronous chess adjudicator

## Description

This is a program to adjudicate blind synchronous chess, an even stricter variant of [Kriegspiel chess](https://en.wikipedia.org/wiki/Kriegspiel_(chess)) where players get no feedback or information whatsoever during the game. As such, games can be played as submissions of two arbitrary-length lists of moves, which are then played by the adjudicator to determine the winner.

## Blind synchronous chess

### Premise

Blind synchronous chess is identical to standard chess, but players cannot see pieces and receive no feedback about game actions. Moves in a game are submitted as lists of chess moves of any length, which an adjudicator attempts to play in the usual alternating fashion. A winner is announced once the adjudicator reconciles the moves.

Moves are resolved to the fullest extent possible, but impossible moves will be interrupted or blocked. A full explanation of resolutions is detailed below.

### Setup and piece movement

Setup and movement of pieces are identical to standard chess. Illegal moves are not permitted.

Moves are submitted in [standard algebraic notation](https://en.wikipedia.org/wiki/Algebraic_notation_(chess)) as specified by FIDE.

### Winning the game

A player wins when:

- they checkmate their opponent, or
- they check their opponent, and their opponent subsequently fails to move out of check, or
- their opponent's move puts their own king into check, or
- their move list is shorter than their opponent's, and both players' moves have been played in their entirety without either player winning.
  - If both move lists are the same length, then black wins by virtue of going second.

### Draws

No draws exist:

- Draws by agreement are not allowed, as moves are predetermined.
- Stalemates and draws on time are resolved when either side makes a move that makes the position into checkmate, or by the move list length rule above.
- Threefold repetition, the 50-move rule, the 75-move rule, and the dead-position rule do not exist in this variant.

### Move resolutions

Moves are resolved to the fullest extent possible. Impossible moves will be interrupted (resulting in a piece possibly moving into an unintended square or performing an unintended capture) or blocked entirely (resulting in a forfeited turn).

#### Move list length imbalances

If one player's move list is longer than their opponent's, once the opponent runs out of responding moves they are assumed to forfeit their subsequent turns.

#### Moving pieces without intent to capture

A piece that moves along rows, columns, or diagonals will attempt to move to the intended square. If its path is uninterrupted, it will move to the intended square. If its path is interrupted by a friendly piece, it will be stopped. If its path is interrupted by an enemy piece, it will capture that piece if legal or will be stopped otherwise (i.e., white playing `e4` with a black pawn on e4 will result in the white pawn stopping at e3).

A piece that moves in other ways (i.e., a knight) will move similarly to the above, but will not move at all if blocked.

#### Capturing

Captures are considered to be identical to moves, as the possible outcomes are the same: a capture if there is an enemy piece on the target square or in the path to it, an interruption if there is a friendly piece in the way, or a block otherwise. Thus the `x` specifier for capturing moves is completely unnecessary.

Pawns that attempt a capture without a valid target present will result in no movement and the player's turn is forfeited.

#### Castling

Identical to standard chess, castling can only be performed if the king and rook have never moved, if the squares between them are not occupied, and if the king is not in check. Castling through check is considered moving into check, and loses the game.

#### Promotion and en passant

Promotion and en passant work the same as in standard chess. If the specified move is not possible, the player's turn is forfeited.

#### Moving unexpected and nonexistent pieces

Since chess notation (usually) only specifies the moving piece and not the exact square of origin, pieces can be successfully moved even when they are not where expected. For example, if a queen on h4 attempts `Qe4` but is blocked by a friendly piece on e4 and ends up on f4, then a subsequent `Qe5` could still be successful.

If no piece can move to satisfy the move, then the player's turn is forfeited.

#### Move specificity and ambiguous move resolution

Much like in regular chess, moves can be written to eliminate ambiguity in case there are multiple pieces that can perform that move. All moves can be written with any (ex. none, rank, file, or square) level of specificity, ex. `Qe1` can be written as `Qhe1`, `Q4e1`, or `Qh4e1`.

Ambiguous moves that can be performed by multiple pieces are considered to be unfulfillable and the player's turn is forfeited.

> Note: in combination with the rules on moving unexpected and nonexistent pieces, this rule adds strategic depth — specific moves may fail if the piece is not where expected, while ambiguous moves may result in unexpected pieces moving or a turn forfeit.

## Strategy

> Note: this is largely conjecture from first principles.

### Defense is paramount

Unlike standard chess or even Kriegspiel chess, there is no way to respond to threats aside from predetermined counterplay. Thus, wildly suicidal attacks can succeed if the attacking piece is not captured or somehow blocked.

### Victory is likely to come from wild guessing

`1. Nc3` and `1. Na3` can lead to potential checkmates from `3. Nc7+`, `3. Nd6+`, or `3. Nf6+`. `1. e4` can lead to `3. Qf7+` or `3. Bxd7+`. A successful defense needs to take those attacking lines into account.

### Knights are unusually powerful

Unlike other pieces, knights cannot be interrupted, only blocked entirely by friendly pieces or captured. Thus in the early game they can be almost unstoppable, as they are almost guaranteed to be exactly where they are played. Capturing knights becomes a guessing game, and creating a wall of pawns cannot protect against mate by knights.

### This game may be completely broken

The whole knight thing might be overpowered. Oops.
